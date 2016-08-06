---
published: false
title: SEH Exception
layout: post
tags: [Exception, ]
---
An exception is an event that occurs during the execution of a program, and requires the execution of code outside the normal flow of control.

OS(Windows) provides a mechanism to handle exceptions known as Structured Exception Handling (SEH). SEH is a mechanism used to handle, and possibly resolve exceptions.  It is used both in User Mode and Kernel mode code. SEH Exceptions, in turn, can be separated into two categories: 

• Hardware Exceptions: Exceptions that are raised in response to process interrupt.  Example of Hardware exceptions are Divide by Zero, Invalid Memory Access. 
(Refer: https://msdn.microsoft.com/en-us/library/w49wew4f.aspx)
Note: Debuggers use hardware exceptions to implement breakpoints and single steeping the target.

• Software  Exceptions: Software Exceptions are triggered by an explicit call to RaiseExcption Win32 API. A C++ throw exception is translated by the compiler into a call implemented in the C runtime library, which finally invokes RaiseException.

Exceptions are connected to entries in the IDT (Interrupt Dispatch Table).


Language-level or framework-level exceptions, such as the C++ or .NET / Common Language Runtime (CLR) exception use SEH to implement for their application-level exception handling mechanisms. 
