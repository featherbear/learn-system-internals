---
title: "Memory Management"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:46+10:00
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
When a program is loaded into memory, the addresses in the program could be overwritten to match the new memory addresses, but it is slow and complicated.

<u>**Idea 2**</u>  
Monitor every memory address request, and add an offset to each address - such that each address exists within the computer memory. However offsets will have to be very well managed and calculated.

{{% /section %}}

---

## Virtual Memory

The concept of virtual memory serves as a solution to several of these program memory concerns

* Arbitrary memory addresses can be used
* The same memory address can be used between processes
* The same memory region can be shared between processes
* Assigned memory does not need to be physically contiguous
<!-- * Program memory can exist outside of the RAM -->

---

![](virtual-memory.png)

Inside the CPU is a Memory Management Unit (MMU).  

This hardware device manages and coordinates access to memory addresses in the system; such as translating virtual memory addresses into physical memory addresses.

<!-- This operation is transparent to the process - it will not know it is accessing a virtual address. -->

---

## Page Tables

{{% section %}}

```text
                        0x00000 +----------+
                                | frame 0  |
                                +----------+
                                | frame 1  |
                                +----------+
        Computer memory ===>    | frame 2  |
                                +----------+
                                | frame 3  |
                                +----------+
                                |   ....   |
                        0x..... +----------+
```

The computer's physical memory is partitioned into blocks of (generally) fixed sizes, each known as a frame. Commonly the frame size is 4 KB.

---

```text
            Page Table                 Memory
            +--------+              +----------+ 0x00000
            | page 0 |+------------>| frame 0  |
            +--------+              +----------+
            | page 1 |+------+     >| frame 1  |
            +--------+        \   / +----------+
            | page 2 |+----------+  | frame 2  |
            +--------+          \   +----------+
            | page 3 |           \  | frame 3  |
            +--------+            \ +----------+
            | page 4 |             >| frame 4  |
            +--------+              +----------+ 0x.....
```

A page table is a mapping of pages to frames.  
i.e <u>`page 2`</u> will map to <u>`frame 1`</u>

---

```text
            Page Table                 Memory
            +--------+              +----------+ 0x00000
            | page 0 |+------------>| frame 0  |
            +--------+              +----------+
            | page 1 |+------+     >| frame 1  |
            +--------+        \   / +----------+
            | page 2 |+----------+  | frame 2  |
            +--------+          \   +----------+
            | page 3 |           \  | frame 3  |
            +--------+            \ +----------+
            | page 4 |             >| frame 4  |
            +--------+              +----------+ 0x.....
```

Page tables remove the requirement for a process to need a contiguous space of memory, which can be difficult to allocate with lots of programs running.

---

### Notes

* Each process has their own page table, which allows the same memory address 'value' / page number to be mapped to a different physical memory frame.

* `PAGE_SIZE == FRAME_SIZE`


{{% /section %}}

---

## Page Table Translation

{{% section %}}

`vaddr -> MMU -> paddr`

When a virtual address is requested by a process, the MMU performs a page table lookup

```c
/******************* C/C++ pseudocode **********************/

#define FRAME_SIZE 4096
#define PAGE_SIZE  PAGE_SIZE

pageNumber   = vaddr / PAGE_SIZE;        // Page number
offset       = vaddr % PAGE_SIZE;        // Address offset
frameNumber  = pageTable[pageNumber];    // Frame number
frameAddress = frameNumber * FRAME_SIZE; // Frame address
paddr        = frameAddress + offset     // Physcal address

// paddr = pageTable[memoryAddress / PAGE_SIZE] * FRAME_SIZE 
//         + memoryAddress % PAGE_SIZE
```

---

Hardware wise, this process is efficient and does not require modulo and division operations

With a page size of 4 KB, there are $2^{12}$ (4096) values.  

_In a 32-bit address space, the last 12 LSB bits are used for the offset, and the first 20 MSB bits are used as the page number_

```
Requested Address: 0x08041234
      addr >> 12          addr & 0xFFF
      1000000001000001    001000110100
      Page: 32833         Offset: 0x234
```

---

![](https://featherbear.cc/UNSW-COMP3231/post/assignment-3/Screenshot%20from%202020-04-15%2018-45-38.png)


{{% /section %}}

---

## Double Page Tables

{{% section %}}
A page table needs to have enough entries to cover the physical address space -> $ \frac{memory}{frame\ size} $

_i.e. with a 32-bit address space, a page table will contain $\frac{2^{32}}{2^{12}} = 2^{20}$ entries_

If each entry was roughly 2 bytes (ie a `short` - 16 bits), the page table will be 2 MB in size; regardless of the number of pages mapped.

<u>Remember</u> - each process has its own page table. 500 process will require 1GB just for the page table!

---

A way to reduce this memory overhead is to introduce a second level of page tables that are only allocated if required.

![](https://web.cs.ucla.edu/classes/spring13/cs111/scribe/15b/2-level_page_table.jpg)

---

![](https://web.cs.ucla.edu/classes/spring13/cs111/scribe/15b/2-level_page_table.jpg)

In the case of a 2-level 10-10 bit page table, the first level PT contains addresses of second-level PTs

---

![](https://web.cs.ucla.edu/classes/spring13/cs111/scribe/15b/2-level_page_table.jpg)

Therefore only $2^{10} \times 2\ (short)$ bytes are required.  
2 KB per process (initially)!

{{% /section %}}

---

## The TLB

<!-- Page Fault -->

{{% section %}}
Inside the MMU, the _Translation Lookaside Buffer_ is a hardware cache of the <!-- 64 --> last used page table entries.  
As the MMU receives a requested virtual memory address, the TLB cache is accessed.

* If a valid entry for the requested memory address exists, it is returned.  
* Otherwise, a "TLB Miss" occurs - which signals the MMU to perform a "TLB Refill"

---

As the TLB is a hardware implementation, its entries are global for all processes - so metadata is required to keep track of which entry is for which process.

* The TLB stores page table entries rather than entire memory addresses with their offsets.  
* In a 32-bit memory address with a 4096 byte page size (12 bit space), the last 12 LSB bits can be used to store these metadata (i.e. process ID, global flag)

{{% /section %}}

---

### TLB Refill

When a TLB Refill occurs, the MMU performs a [page table lookup](#/6) to find the correct page and frame.

If a page has not been accessed before, a frame will be allocated, and the page will be mapped to that frame.

An entry will then be stored in the TLB, containing the requested page, the resolved frame, and the process metadata. If the TLB is full, an entry is replaced (either by random, or by last-recently-used)

Note: Several pages _can_ map to the same frame

---

## Swapping

When the RAM is full, and more memory is needed, the computer can use space on secondary storage (i.e. from an SSD) as a RAM storage location. However, as secondary storage is logically further away from the CPU, it will be much slower.

Regardless, it allows for the memory of inactive processes to be stored off-RAM, to create more space for new/active processes that request for memory.

---

## Recap

* Arbitrary memory addresses can be used
* The same memory address can be used between processes
* The same memory region can be shared between processes
* Assigned memory does not need to be physically contiguous
* Program memory can exist outside of the RAM

---

## OS/161 Memory

![](https://featherbear.cc/UNSW-COMP3231/post/assignment-3/sketch.png)
