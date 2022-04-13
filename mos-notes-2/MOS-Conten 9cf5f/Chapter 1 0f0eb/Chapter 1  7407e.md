# Chapter 1.1: What is an Operating System?

It is difficult to precisely define an OS. Usually, it runs in kernel mode, but even this isn’t always true.

This is because it performs two functions that are essentially unrelated:

- Provides application programmers a clean, abstract set of resources
- Managing the messiness of the underlying hardware resources

## The Operating System as an Extended Machine

The **architecture** of most computers (instruction set, memory organisation, I/O, and bus structure) at the machine-language level is primitive and difficult to work with. A 2007 book describing an early version of the interface to SATA disks was over 450 pages long, and the standard has only been revised and built upon since then.

Instead of necessitating the programmer to learn these machine-level details, a piece of software called a **disk driver** hides the details and provides an interface that makes it easier to write and read blocks. There are many different drivers for different I/O devices in an OS.

Although this abstraction makes it slightly easier to work with, even this level is too difficult for most developers to work with. For this reason, programmers are provided an additional level of abstraction: **files**. This allows programs to create, write and read files, without needing to worry about the messy world of drivers and machine language.

Abstraction is the key to managing complexity. Good abstractions turn a difficult task into two more manageable ones:

<aside>
💡 In the top-down view, the job of an operating system is to create, implement and manage abstract objects.

</aside>

- Defining and implementing abstractions
- Using these abstractions to solve the problem at hand.

It’s worth noting that the real clients of the OS are application programs. These programs deal directly with the abstractions of the operating system. In contrast, the end users of applications deal with yet another abstraction, that of the user interface - this could be either CLIs or GUIs. In this book, we talk frequently about abstractions provided to application programs, but little about user interfaces.

## The Operating System as a Resource Manager

The concept of the OS primarily producing abstractions to programs is a top-down view. In contrast, a bottom-up view holds that the OS primarily exists to **manage all the complex moving parts**. According to this view, the job of the OS is to provide orderly allocation of processors, memory, and I/O devices amongst the various programs that want them.

In short, this view holds that the OS’ primary task is to keep track of which programs are using which resources, grant resource requests, account for usage, and resolve conflicting requests from different programs and users.

<aside>
💡 In the bottom-up view, the job of an operating system is to allocate resources amongst all the programs that need them.

</aside>

Resource management involves **multiplexing** (sharing) resources in **time** and in **space**. If a resource is time-multiplexed, different users take turns using it. Determining how long to time-multiplex a resource is the OS’ job. On the other hand, if a resource is space-multiplexed, each user gets to use one part of the resource. Main memory, for instance, is usually divided up between multiple programs so each can exist at the same time. Assuming an adequate amount of memory, it is more efficient to space-multiplex than to time-multiplex, especially if the program only needs a small amount of the total.

<aside>
💡 Resources can be time-multiplexed or space-multiplexed.

</aside>

Another space-multiplexed resource is the disk - in many systems, a disk can hold files from many users at the same time. Allocating disk space and keeping track of who is using which disk block is a typical OS task.

---

[← Chapter 1](Chapter%201%20%20a48da.md)

[Home](../../AiredDev%20b02d5/Notes%20on%20M%2061e3e.md)

[Chapter 1.2 →](Chapter%201%20%20984da.md)