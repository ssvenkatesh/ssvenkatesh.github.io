---

title: How Exceptions are implemented in Windows?
date:   2016-08-06 21:45:15 +0530
layout: post
tags: [Exception,Windows, C++ ]
---

An exception is an event that occurs during the execution of a program, and requires the execution of code outside the normal flow of control.

OS(Windows) provides a mechanism to handle exceptions known as Structured Exception Handling (`SEH`). `SEH` is a mechanism used to handle, and possibly resolve exceptions.  It is used both in User Mode and Kernel mode code.

`SEH` Exceptions, in turn, can be separated into two categories: Hardware and Software Exceptions.

**• Hardware Exceptions:** Exceptions that are raised in response to process interrupt.  Example of Hardware exceptions are Divide by Zero, Invalid Memory Access.  [Hardware Exceptions](https://msdn.microsoft.com/en-us/library/w49wew4f.aspx "Hardware Exceptions")
Note: Debuggers use hardware exceptions to implement breakpoints and single steeping the target.

Exceptions are connected to handlers using Interrupt Dispatch Table (IDT). Entries in the IDT are shown below. Each entry in the table corresponds to a hardware interrupts and handler. Interrupt 3 is a debugging breakpoint interrupt and BreakPointHander is invoked.

```
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
```

**• Software  Exceptions:** Software Exceptions are triggered by an explicit call to RaiseExcption Win32 API. A C++ throw exception is translated by the compiler into a call implemented in the C runtime library, which finally invokes RaiseException.  


{% highlight cpp %}
void foo()
{
	throw;
}
int _tmain(int argc, _TCHAR* argv[])
{
	foo();
	return 0;
}
{% endhighlight %}


**Stack frame of software exception in Windebug**
```
0:000> k
ChildEBP RetAddr  
008af840 511b0b86 KERNELBASE!RaiseException+0x48
008af88c 012b13e7 MSVCR120D!_CxxThrowException+0x96 [f:\dd\vctools\crt\crtw32\eh\throw.cpp @ 154]
008af968 012b1433 ConsoleApplication4!foo+0x27 [c:\consoleapplication4\consoleapplication4\consoleapplication4.cpp @ 9]
008afa3c 012b1979 ConsoleApplication4!wmain+0x23 [c:\consoleapplication4\consoleapplication4\consoleapplication4.cpp @ 14]
008afa8c 012b1b6d ConsoleApplication4!__tmainCRTStartup+0x199 [f:\dd\vctools\crt\crtw32\dllstuff\crtexe.c @ 623]
008afa94 7628495d ConsoleApplication4!wmainCRTStartup+0xd [f:\dd\vctools\crt\crtw32\dllstuff\crtexe.c @ 466]
008afaa0 774a98ee KERNEL32!BaseThreadInitThunk+0xe
008afae4 774a98c4 ntdll!__RtlUserThreadStart+0x20
008afaf4 00000000 ntdll!_RtlUserThreadStart+0x1b
```


`SEH` Exceptions and their structures are used to implement  try, catch and finally blocks in your code. Unlike C++ exceptions and .NET Exceptions, which can be raised with any type, `SEH` exceptions deal with only one type.  i.ie., unsigned int. ( Exception codes by OS).

Language-level or framework-level exceptions, such as the C++ , Java,  .NET / Common Language Runtime (CLR) exception use `SEH` to implement for their application-level exception handling mechanisms internally. 




