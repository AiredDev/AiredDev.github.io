# Chapter 1: Introduction

An operating system is a layer of software responsible for providing user programs with a simpler, cleaner model of the computer and to handle management of resources.

The program that a user interacts with is not typically part of the operating system, although it uses the OS to get its work done.

A high-level illustration of a computer is shown below:

<aside>
💡 If the interface is text-based, it can be called a shell; if it has icons it can be called a GUI (**G**raphical **U**ser **I**nterface).

</aside>

![Untitled](Chapter%201%20%20a48da/Untitled.png)

The hardware consists of chips, boards, disks, keyboards, monitors, often mice, and other physical objects.

Layered atop the hardware we see the operating system.

A layer above the operating system we see the user interface program, and regular applications like web browsers above that.

Also in the diagram, we see two distinct "operating modes" of a computer: **kernel mode** and **user mode**. These two modes are consistently found in most computers.

The operating system runs in kernel mode (also called **supervisor mode**), where it has absolute control over the hardware, and can execute any instruction the hardware is capable of executing.

Other software, meanwhile, run in user mode, in which only a subset of the instructions are available. In particular, user-mode programs are forbidden from executing instructions that affect control of the computer or do I/O.

The division between kernel mode and user mode play a crucial role in how OSes work.

We see in the diagram that the OS runs atop bare hardware, providing the basis for all other programs.

Sometimes, the distinction between the OS and userspace programs can blur, for example on embedded systems that may not have kernel mode.

Operating systems also differ from userspace in other ways. They are huge, complex, and long-lived software systems, with the kernel often approaching 5 million lines of code or more. When libraries are included, that number can inflate to 70 million. Because of the pure scope of these programs, OSes usually evolve over time rather than being fully replaced. Windows versions NT, 2000, XP, Vista, 7, 8 and 10 are all, essentially, a single operating system.

---

[← Home](../../AiredDev%20b02d5/Notes%20on%20M%2061e3e.md)

[Home](../../AiredDev%20b02d5/Notes%20on%20M%2061e3e.md)

[Chapter 1.1 →](Chapter%201%20%207407e.md)