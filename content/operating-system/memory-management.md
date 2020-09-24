---
title: "Memory Management"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:46+10:00
draft: true
tileColour: "#D7A9E3"
---

{{< slide class="center" >}}

## Memory Management

---

## Memory Management

As covered in [CPU Fundamentals](../../cpu-fundamentals/memory/#/4), registers are very fast but very limited in capacity, whereas secondary storage devices (i.e. a hard disk) hold large amounts of data at the expense of speed

* Registers - ~1 ns.
* Caches - ~2 ns.
* Main memory (RAM) - ~10 ns.
* Magnetic disks - ~10 ms.

<u>**Principle of Locality**</u>  
Things closer to each other are faster

---

## Program Memory Binds

{{% section %}}
> Before a program is executed, it is loaded into memory

During program compilation, various addresses are hardcoded into the program - but there is no guarantee that those addresses are available or exist

---

<u>**Idea 1**</u>  
When a program is loaded into memory, the addresses in the program could be overwritten in to match the new memory addresses, but it is slow and complicated.

<u>**Idea 2**</u>  
Monitor every memory address request, and add an offset to each address - such that each address exists within the computer memory. However offsets will have to be very well managed and calculated.

{{% /section %}}

---

## Virtual Memory

The concept of virtual memory serves as a solution to program memory concerns

* Arbitrary memory addresses can be used
* The same memory address can be used between processes
* Actual memory can be shared between processes
* Program memory can exist outside of the RAM

---

* Inside the CPU is a Memory Management Unit (MMU) - which manages and coordinates access to memory addresses in the system.



* The MMU contains a cache known as the Translation Lookaside Buffer (TLB)

MMU
TLB  - page fault
Page Table

## Page Tables



## Memory Allocation

### Fragmentation

* External Fragmentation
* Internal Fragmentation
* Static Memory Management
  * Simple
  * First Fit
  * Any Fit
* Dynamic Partitioning
  * First Fit
  * Next Fit
  * Best Fit

## Swapping

When the RAM is full, and more memory is needed, the computer can use space on secondary storage (i.e. from an SSD) as a RAM storage location. However, as secondary storage is logically further away from the CPU, it will be much slower.

Regardless, it allows for the memory of inactive processes to be stored off-RAM, to create more space for new/active processes that request for memory.


<!-- https://featherbear.cc/UNSW-COMP3231/post/virtual-memory-management/ -->
<!-- https://featherbear.cc/UNSW-COMP3231/post/translation-lookaside-buffer/ -->
<!-- https://featherbear.cc/UNSW-COMP3231/post/virtual-memory/ -->
<!-- https://featherbear.cc/UNSW-COMP3231/post/memory-management/ -->
<!-- https://featherbear.cc/UNSW-COMP3231/post/memory/ -->

# Stack and Heap Allocation