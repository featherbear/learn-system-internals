---
title: "Access Management"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:40+10:00
tileColour: "#D7A9E3"
---

{{< slide class="center" >}}

## Access Management

---

## Processer Privilege Levels

{{% section %}}

Some microprocessors are designed to operate under different privilege levels.  

In **User / Application Mode**, resources such as devices, registers, certain memory regions and certain CPU instructions are restricted.  

In **Kernel / Privileged Mode**, full access to resources and CPU instructions are permitted.

_On such systems, the kernel runs in privileged mode, whilst most other applications run in user mode._

---

If an application running inside user mode attempts to access a restricted memory region, or attempts to execute a restricted CPU instruction, the request will be denied.

Privileged processors enforce the restrictions and policies of the operating system on applications, as only the kernel has privileged access. This helps to mitigate the risk of malicious / unexpected application behaviour from affecting the rest of the system.

---

{{% note %}}

Draw a diagram.

{{% /note %}}

```plain
                                       +---------------+
                                       |               |
        +---------+    user access     |               |
        |         +------------------->|               |
        |  App    ++                   |      CPU      |
        +---------+ \                  |               |
                     \                 |               |
                      \                +------------+--+
                       v                            |
                  +-----------+                     |
                  | Operating |     privileged      |
                  |   System  +---------------------+
                  +-----------+       access
```

{{% /section %}}

---

## Privilege-less Processors

Some microprocessors do not implement a privilege level system, due to the complexity and cost of the circuitry required.

As no level check is required, privilege-less processors run considerably faster than privileged processors, and thus are often used in embedded devices, where small and single tasked operations are required.

Whilst they can implement the functionality of the OS, they are unable to enforce restrictions and policies. (You can bypass the OS)

---

## System Calls

{{% section %}}

Application access to the kernel functionality is provided through 'system call requests', also known as `syscalls`.  

When a `syscall` is made from a program, the processor switches to the operating system (kernel mode) to handle the request (or not).  
When the routine is finished, the processor switches back to the execution of the program

[[x86 syscall table](https://filippo.io/linux-syscall-table/)]

{{% note %}}
Syscalls complement the implementation of privileged CPU systems
{{% /note %}}

---

Syscall requests are constructed in the program by setting the various CPU registers to certain values. _Then_, when the `syscall` CPU instruction is executed, the CPU will perform a `kernel trap` - switching to the execution of the operating system in kernel mode.

* Push relevant arguments to the stack
* Set up relevant registers
* Execute syscall
* ...
* Return from syscall

---

The CPU often contains an [interrupt controller](../cpu-fundamentals/waiting-for-events/#/3) which on top of its 'interrupt controlling duties', also contains parts of the processor state prior to an interrupt being triggered.

For example - The EPC (Exception Program Counter) contains the address that triggered the syscall

---

The implementation of a syscall is defined by the OS.  
As the operating system runs in kernel mode, it has full access to the entire range of CPU instructions and memory space.

{{% /section %}}