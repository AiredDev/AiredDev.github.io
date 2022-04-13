# Chapter 1.3: Computer Hardware Review

A simple PC can be conceptually abstracted into the following diagram:

![Untitled](Chapter%201%20%20a7335/Untitled.png)

The CPU, memory and I/O devices are all connected by a system bus and communicate with one another over it.

Modern devices are more complex, with multiple buses; we will look at this later.

## Processors

The CPU is the “brain” of the computer. It fetches instructions from memory and executes them.

The basic cycle of every CPU is to **fetch** the next instruction from memory, **decode** it (determining type and operands) and then **execute** it. The cycle is repeated until the program finishes.

Each CPU has a certain set of instructions that it can execute. Therefore an ARM processor cannot run an x86 program, nor can an x86 program run an ARM one.

Because accessing memory to get an instruction or data takes much longer than to execute, every CPU has a small set of **registers** that can hold key variables and temporary results. The instruction set of each processor generally contains instruction to load a word into a register from memory, and to store a word into memory from a register. Other instructions may combine two operands from registers, memory or both into a result, for example adding or subtracting two words and storing the result into another register.

In addition to these general registers for temporary values, most processors have several special-purpose registers that are visible to the programmer. One of these registers is the **program counter**, which contains the memory address of the next instruction to be executed. Another special register is the **stack pointer** which points to the top of a stack currently in memory. The stack contains one **frame** for each procedure that has been entered but not yet exited. The stack frame contains those input parameters, local variables and temporary variables that are not stored in registers. A third special-purpose register is the **PSW (program status word)**. This contains special bits which are set by the computer. The PSW can contain condition code bits from comparisons, the CPU priority, the mode (user / kernel), and various others. Normally, user programs can read the entire PSW but may only be able to write to some of them. This register plays an important role in I/O and in system calls.

The operating system needs to be fully aware of all registers at all times. When time multiplexing the CPU, the OS will often stop one running program and start another one. Each time it stops a running program, the values of all the registers at that point in execution must be saved so they can be restored later.

To improve performance, many CPU designs have long abandoned the simple one-at-a-time fetch-decode-execute cycle. Some CPUs, for example, may have separate units for fetching, decoding and executing, so that it can execute one instruction while decoding another and fetching a third. This sort of organisation is called a **pipeline**. A pipeline with 3 stages is illustrated below.

![Untitled](Chapter%201%20%20a7335/Untitled%201.png)

Pipelines longer than three stages are common. In most pipeline designs, once an instruction has been fetched it must be executed, even if the preceding instruction was a conditional branch that was taken.

An even more advanced design than a pipeline is a **superscalar CPU**, shown below.

![Untitled](Chapter%201%20%20a7335/Untitled%202.png)

In this design, there are multiple execution units present. One unit, for example, may be for integer arithmetic, another for floating-point arithmetic, and a third for Boolean operations. Two or more instructions may be fetched and decoded at the same time, then placed into a holding buffer until they can be executed. As soon as an execution unit becomes available it looks into the holding buffer for an instruction it can handle.

An implication of the superscalar design is that programs are often executed out of order. It is mostly up to the hardware to make sure the produced result is the same as if it were executed in order, but there is a large amount of responsibility placed on the OS too, as we will see later.

Most CPUs, other than very simple ones in embedded systems, have two modes: **kernel** mode and **user** mode. This is usually controlled by a bit in the PSW. When running in kernel mode, the CPU can execute every instruction in its instruction set and use every feature of the hardware. On the other hand, user mode programs only allow for a subset of instructions and features to be used. Generally, instructions related to I/O and memory protection are disallowed in this mode. It is also forbidden to set the bit controlling the mode.

On desktops and servers, the OS usually runs in kernel mode. On embedded systems, only a small piece runs in kernel mode, with the rest of the OS running in user mode.

To obtain services from the OS, a user program must make a **system call**. The instruction handling system calls switches from user mode to kernel mode, starts the operating system, and then returns control to the user program once the necessary work has been completed. Details of this mechanism are explained later.

## Multithreaded and Multicore Chips

According to Moore’s Law, the number of transistors on a chip doubles every 18 months. Eventually, Moore’s Law will no longer hold as the number of atoms per transistors become so small that quantum mechanical effects become significant enough to prevent further transistor shrinkage.

The sheer number of transistors is beginning to pose a problem: what do we do with them all? One approach is to produce superscalar PCs as illustrated above. 

Another approach is to create larger processor registers. This is happening, but eventually we’ll have diminishing returns.

The next step is to replicate not only functional units, but some of the control logic as well. Intel’s Pentium 4 processor introduced this property, called **multithreading** or **hyperthreading**. Several other chips are multithreaded, including the SPARC, the Power5, the Intel Xeon and the Intel Core family.

<aside>
💡 Each thread in a multithreaded CPU replicates some of the control logic of a CPU.

</aside>

Multithreading allows the CPU to hold the state of two or more threads and switch back and forth between them on the scale of nanoseconds. A thread is a type of lightweight process which in turn is a running program; the meaning of threads is clarified in Chapter 2. For example, if one process needs to read a word from memory, taking multiple clock cycles, multithreaded CPUs can just switch to another thread.

Multithreading does not allow for true parallelism: only one thread runs at any one time, but it approaches something that looks like parallelism because of how little time it takes to switch between threads.

<aside>
💡 Multithreading does not allow for true parallelism.

</aside>

To the OS, each thread is a separate CPU. If we have two physical CPUs, each of which have two threads, for example, the OS will see 4 CPUs. However, if there is only enough work to keep two CPUs busy at some point in time, it may inadvertently schedule two threads on the same CPU while the second CPU stays idle. This is far less efficient than using the two separate physical CPUs.

Beyond multithreading, many CPU chips now have multiple **cores** on them. The below figure illustrates how a four-core CPU effectively has four different minichips on it, each of which have their own independent CPUs.

![Untitled](Chapter%201%20%20a7335/Untitled%203.png)

Modern GPUs are similar to multicore CPUs. However, GPUs have literally thousands of of tiny cores. This makes them good for parallel computing tasks, like drawing polygons on a screen, but are not good at serial computing. They are also difficult to program.

GPUs can be useful for some OS tasks like encryption and network traffic processing, but it’s unlikely that a lot of the OS will run on the GPU.

## Memory

Memory is the second major component in any computer. An idealised model of memory would be one that runs faster than the execution of any CPU instruction (so that the CPU is not held up by memory), very large, and very cheap. Unfortunately, no such technology currently exists, so a different approach is taken.

Memory is constructed as a hierarchy of different layers, shown below. The top layers have higher access speed, but smaller capacity, and greater cost per bit. The lower layers have lower access speed, but larger capacity and less cost per bit.

![Untitled](Chapter%201%20%20a7335/Untitled%204.png)

### Registers

The top layer consists of **registers** internal to the CPU. They are made of the same material as the CPU and are thus just as fast. Typically, their storage capacity is about 32 $\times$ 32 bits on a 32-bit CPU, and 64 $\times$ 64 bits on a 64-bit CPU. In both cases, this is less than 1KB.

### Caches

The next layer is **cache** memory. Caches are mostly controlled by the hardware. Main memory is divided into **cache lines**, typically 64 bytes each. The first cache line contains bits 0 to 63, the second contains bits 64 to 127, and so on. The most heavily-used cache lines are stored in the cache layer located inside or very close to the CPU. When the program needs to read a word from memory, cache hardware first checks whether the requested cache line is in the cache. If it is, it’s called a **cache hit**; otherwise, it’s called a **cache miss**. In the case of a cache hit, the request is satisfied and no memory request needs to be sent over the bus to main memory. They usually take about two clock cycles. Cache hits, on the other hand, have a significant time penalty.

<aside>
💡 Main memory is divided into cache lines, each typically 64 bytes long.

</aside>

Caching plays a major role in many areas of computer science. Whenever a resource can be divided into pieces, some of which are used more heavily than others, caching can be utilised to improve performance.

In any caching system, several questions come up fairly soon, such as:

- When to put a new item into cache
- Which cache line to put the new item in
- Which item to remove from the cache when a slot is needed
- Where to put a removed cache item in main memory

However, not every question is relevant to every caching situation. For caching lines of main memory in the CPU cache, a new item usually gets inserted whenever a cache miss occurs. The used cache line is usually computed by using some of the high-order bits of the memory address referenced. Finally, when a cache line is rewritten to main memory, the place in memory to rewrite it to is uniquely determined by the address in question.

Modern CPUs have two levels of cache. The first level, called the **L1 cache**, is always inside the CPU. It usually feeds decoded instructions to the CPU’s execution engine. Most chips have an additional L1 cache for heavily-used words. L1 caches are usually 16 KB each.

The second level, called the **L2 cache**, usually holds several megabytes of recently-used words.

The difference between the L1 and L2 caches are in the timing. L1 caches are accessed without delay; L2 caches are accessed in one or two clock cycles.

On multicore chips, the designers need to decide where to put the caches. Sometimes, one L2 cache is shared by all the cores; this approach is used by Intel chips. In contrast, some chips have their own, smaller L2 caches for every core; this approach is used by AMD. Both approaches are shown below.

![Untitled](Chapter%201%20%20a7335/Untitled%205.png)

Each approach has its pros and cons. For example, a shared L2 cache requires a more complicated cache controller, while separate L2 caches make it difficult to keep the caches consistent.

### Main memory

Main memory is the workhouse of the memory system. It is usually called **RAM** (random access memory). Older programmers might also call it **core memory**.

Currently, main memory is many hundreds of megabytes to gigabytes large, and growing rapidly. All CPU requests that cannot be satisfied by the cache go to main memory.

In addition to main memory, modern computers usually have a small amount of non-volatile RAM, called **ROM** (read-only memory). This kind of memory is cheap and fast, and doesn’t lose its contents when the power is switched off. It is programmed at the factory and cannot be changed afterwards. On some computers, the bootstrap loader which starts the computer is stored in ROM. Some I/O cards come with ROM for handling low-level device control.

**EEPROM** (electrically erasable programmable read-only memory) and **flash memory** are other non-volatile forms of memory, but in contrast to ROM can be erased and rewritten. However, writing to these forms of memory takes much more time than RAM, so they’re used in much the same way as ROM with the additional feature of being programmable if they need to be (for example correcting bugs).

Flash memory is also commonly used as a storage medium in portable electronic devices. It can serve as film for digital cameras, or the disk in portable music players, for example. This form of memory is intermediate in speed between RAM and the disk. Unlike disk memory, it wears out if erased too many times.

Yet another kind of memory is **CMOS** (complementary metal-oxide semiconductor). CMOS is volatile. Many computers use CMOS to hold the current time and date. The CMOS memory and the clock circuit that increments the time are powered by a small battery, ensuring the time is correctly updated even when the computer is unplugged. CMOS can also hold configuration parameters, such as which disk to boot from. It is often used because it draws so little power that the original factory-installed battery often lasts several years.

### Disks

Next in the hierarchy is the magnetic disk. Disks are many times cheaper per bit than RAM. It is usually many times larger as well. The only problem with disk memory is how long it takes to randomly access data on it. The reason is that the disk is a mechanical device. A schematic of a hard disk drive is shown below.

![Untitled](Chapter%201%20%20a7335/Untitled%206.png)

A hard disk drive consists of one or more metal platters that rotate at 5400, 7200, 10800, or sometimes more RPM. A mechanical arm pivots over the platters from the corner, similar to the arm on an old vinyl player. 

Information is written onto the disk in a series of concentric circles. At any given arm position, each head can read a ring-shaped region called a **track**. All the tracks at the same radial difference from the centre, across all platters, is called the **cylinder**. Each track is divided into some number of **sectors**, typically 512 bytes each. On modern disks, the outer cylinders contain more sectors than the inner cylinders.

![Untitled](Chapter%201%20%20a7335/Untitled%207.png)

![Untitled](Chapter%201%20%20a7335/Untitled%208.png)

Moving the arm from one cylinder to the next takes about 1 millisecond. Moving onto a random cylinder takes between 5 and 10 milliseconds, depending on the drive.

Once the arm is on the correct track, the drive needs to wait for the the necessary sector to rotate under the head, which also takes 5-10 ms, depending on the drive’s RPM.

Once the sector is under the head, bits are read or written at a rate between 50 MB/s on the low end, and 160 MB/s on the high end.

In recent years, **SSDs** (solid-state drives) have increasingly come to the market as disk memory. Despite their position in the memory hierarchy, SSDs are actually not disks at all. They do not have any moving parts or platters and they store data via flash memory. The main similarity between SSDs and HDDs is that SSDs can store a lot of data which isn’t lost when power is turned off. Because of the lack of moving parts, SSDs are much faster than hard drives, but their cost per bit is still much higher.

Many computers support an abstraction called **virtual memory**, which will be covered in depth in Chapter 3. This scheme makes it possible to run programs larger than physical memory by placing them on the disk and using main memory as a kind of cache for the most heavily-executed parts. Virtual memory requires remapping memory addresses on the fly to convert the address the program generates to the physical address in RAM where the word is located. The mapping is done by a dedicated part of the CPU called the **MMU** (memory management unit).

Caching and the presence of the MMU can have a major impact on performance. In a multiprogramming system, when switching from one program to another (sometimes called a **context switch**), it may be necessary to flush modified blocks from the cache and change the mapping registers on the MMU. Both are expensive operations, and so it’s important for programmers to avoid them wherever possible. Some implications of this avoidance is covered later.

## I/O Devices

The CPU and memory are not the only resources managed by the OS. I/O devices also interact heavily with the OS.

I/O devices generally consist of two parts: a **controller**, and the **device** itself.

The controller is a chip or a set of chips that physically controls the device. It accepts commands from the OS to, for example, read data from the device, and carries the commands out. In many cases, the actual control of the device is very complex and detailed, so the job of the controller is to present a simpler, albeit still complicated, interface to the operating system. For example, a disk controller might accept a command to read sector 11206 from disk 2. The controller would then convert this linear sector number to a cylinder, sector and head. The conversion may be complicated by the fact that outer cylinders have more sectors than inner ones, or that some bad sectors have been remapped onto other ones, for example. Then, the controller has to determine which cylinder the disk arm is on and give it a command to move in or out the necessary number of cylinders. It would have to wait until the proper sector has rotated under the head and then start reading and storing the bits as they come off the drive, removing preamble and computing checksum. Finally, it has to assemble bits into words and store them in memory. To do all this work, controllers often contain small embedded computers that are programmed to do the work for them.

The device itself usually has a comparatively simple interface, both because they don’t do much and so they can be standardised. Standards are needed so that any SATA controller can handle any SATA disk, for example.

**SATA** (Serial AT Attachment) is currently the standard type of disk on many computers. Since the actual device is hidden behind the controller, all the OS sees is the interface to the controller; this may be significantly different than the controller’s interface to the device.

Because each type of controller is different, different software is needed to control each one. The software that talks to a controller, giving commands and accepting responses, is called the **device driver**. Each controller manufacturer has to supply a driver for each operating system it supports. A scanner, for example, may come with drivers for OS X, Windows 7, Windows 8, Windows 10 and Linux.

To be used, the driver has to be put into the OS so it can run in kernel mode. It is possible for drivers to run outside kernel mode, and modern Windows and Linux OSes provide some support for this, but the vast majority of drivers still run in kernel mode. Very few current systems run all drivers in userspace. When they are run in userspace, drivers must be allowed to access the device in a controlled way, which is not straightforward.

The driver can be put into kernel space in 3 ways:

- relink the kernel with the new driver and restart the system
    - This is an older technique, used by some old UNIX systems
- make an entry in an OS file telling it that it needs the driver, and then restart the system
    - Windows works this way
    - At boot time, the OS finds the drivers it needs and then loads them
- Have an OS capable of accepting new drivers while running, and install them on the fly without a need to reboot
    - Used to be quite rare, but is becoming much more common
    - Hot-pluggable devices such as USBs and IEEE 1394 devices always require dynamically-loaded drivers

Each controller has a small number of registers that are used to communicate with it. For example, a minimal disk controller might have registers for specifying the disk address, memory address, sector count, and direction (read/write). To activate this controller, the driver gets a command from the OS, then translates it into the appropriate values to write into device registers.

The collection of all the device registers forms the **I/O port space**, which will be covered in Chapter 5.

---

[← Chapter 1.2](Chapter%201%20%20984da.md)

[Home](../../AiredDev%20b02d5/Notes%20on%20M%2061e3e.md)

[Chapter 1.4 →](Chapter%201%20%2077857.md)