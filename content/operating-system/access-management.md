---
title: "Access Management"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:40+10:00
draft: true
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

{{% /section %}}

---

## Privilege-less Processors

Some microprocessors do not implement a privilege level system, due to the complexity and cost of the circuitry required.

As no level check is required, privilege-less processors run considerably faster than privileged processors, and thus are often used in embedded devices, where small and single tasked operations are required.

Whilst they can implement the functionality of the OS, they are unable to enforce restrictions and policies. (You can bypass the OS)

---

## System Calls

Access to the kernel's functionality is provided through system call requests, also known as `syscalls`.  

When a `syscall` is made from a program, the processor switches to kernel to handle the request.  