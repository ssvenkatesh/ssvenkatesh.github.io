---
published: false
title: SEH Exception
layout: post
tags: [Exception, ]
---

An exception is an event that occurs during the execution of a program, and requires the execution of code outside the normal flow of control.

OS(Windows) provides a mechanism to handle exceptions known as Structured Exception Handling (SEH). SEH is a mechanism used to handle, and possibly resolve exceptions.  It is used both in User Mode and Kernel mode code.

SEH Exceptions, in turn, can be separated into two categories:  Hardware and Software Exceptions.

• Hardware Exceptions: Exceptions that are raised in response to process interrupt.  Example of Hardware exceptions are Divide by Zero, Invalid Memory Access. 
(Refer: https://msdn.microsoft.com/en-us/library/w49wew4f.aspx)
Note: Debuggers use hardware exceptions to implement breakpoints and single steeping the target.

Exceptions are connected to handlers using Interrupt Dispatch Table (IDT). Entries in the IDT are shown below. Each entry in the table corresponds to a hardware interrupts and handler. Interrupt 3 is a debugging breakpoint interrupt and BreakPointHander is invoked.



kd> !idt -a

Dumping IDT: 80b95400

00:   8464d200 nt!KiDivideErrorFault
01:   8464d390 nt!KiTrap01
02:   Task Selector = 0x0058
03:   8464d800 nt!KiBreakpoint Trap
04:   8464d988 nt!KiTrap04
05:   8464dae8 nt!KiTrap05
06:   8464dc5c nt!KiTrap06
07:   8464e258 nt!KiTrap07
08:   Task Selector = 0x0050
09:   8464e6b8 nt!KiTrap09
0a:   8464e7dc nt!KiTrap0A
0b:   8464e91c nt!KiTrap0B
0c:   8464eb7c nt!KiTrap0C
0d:   8464ee6c nt!KiTrap0D
0e:   8464f51c nt!KiTrap0E
0f:   8464f8d0 nt!KiTrap0F
10:   8464f9f4 nt!KiTrap10
11:   8464fb34 nt!KiTrap11



• Software  Exceptions: Software Exceptions are triggered by an explicit call to RaiseExcption Win32 API. A C++ throw exception is translated by the compiler into a call implemented in the C runtime library, which finally invokes RaiseException.
