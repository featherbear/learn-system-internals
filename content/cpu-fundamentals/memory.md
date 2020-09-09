---
title: "Memory"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:33+10:00
tileColour: "#8BBEE8"

---

{{< slide class="center" >}}

# Memory 

---

## Everything is Memory

{{% fragment %}}
When we save a file, it is saved to the computer memory.
{{% /fragment %}}

{{% fragment %}}
When we perform an arithmetic operation, its operands and outputs are stored in the memory
{{% /fragment %}}

{{% fragment %}}
When we press a key, a flag in the memory is changed.
{{% /fragment %}}

{{% fragment %}}
When we launch a program, that program is loaded into memory!
{{% /fragment %}}

---

## Endian-ness

{{% section %}}

Multi-byte values (i.e. values greater than 255) can be stored to the memory in two different ways.

* Big Endian - The MSB (most significant byte) is first
  * i.e. The number `287` is stored as `\x01\x1f`
* Little Endian - The LSB (least significant byte) is first
  * i.e. The number `287` is stored as `\x1f\x01`

---

### Uses

Most systems use the Little Endian address order, due to its inherent properties

* e.g faster operations
  * Checking if an `int` (4 bytes) at `0x11223344` is even, only that address needs to be checked
    * If Big Endian is used, the CPU will need to calculate and then check `0x11223347`

---

### Uses

Most systems use the Little Endian address order, due to its inherent properties

* e.g type casting
  * Casting an `int` (4 bytes) to a `char` (1 byte) is trivial, as the LSB is already at the right address

> The only common modern-day use for Big Endian is for storing strings

{{% /section %}}

---

## RAM

Random Access Memory is a form of computer memory that allows data to be quickly written and read. They are (relatively) cheap, and complement the CPU registers by holding large amounts of data that is frequently accessed.

It is called Primary Storage - as it is connected directly to the CPU. Secondary storage devices (i.e. hard disks) are connected indirectly.  

RAM is considered volatile / non-persistent memory - data is only kept when the RAM is powered on.

---

## Registers vs RAM

||Registers|Primary Storage (RAM)|Secondary Storage (SSD/HDD)|
|:---|:---:|:---:|:---:|
|Location|CPU|On-board|External|
|Speed|Fast|Slow|Slower|
|Price|-|$$|$|
|Capacity|bytes|gigabytes|_a lot_|

***Registers are fast but they cannot store a lot of data!***

---

## Memory Mapped I/O

Both registers and RAM modules store data, however due to their different locations - they are accessed in different ways.  

Some CPU architectures allow "Memory Mapped IO" operation, which can map various I/O devices (including registers!) to virtual locations in the memory.

Therefore, the access methods to register data and RAM data can be unified, making interoperability better.

---

## Port Mapped I/O

Like memory mapped I/O, output ports of devices in a system can also be mapped to virtual addresses in the memory

---

## Memory Layout

![](https://i.stack.imgur.com/oETv1.jpg)

As mentioned earlier, when a program is executed, its instructions are copied into the memory.  
By standard, the instructions are loaded near the start (lower portion) of the available memory space.

---

### Stack

{{% section %}}

The _stack_ contains the temporary memory and data used by the functions of a program, and is located near the top of the memory address space.

It is called a stack due to how it is accessed.

* When a function is called a **stack frame** is created and appended to the end of the stack.
* When that function exits, its stack frame is removed from the stack frame.

---

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Call_stack_layout.svg/513px-Call_stack_layout.svg.png)

---

The stack frame contains the parameters and temporary variables that a function will use to complete its execution.  

The stack frame also contains data related to what the CPU should do after the function finishes, such as values of the CPU registers prior to the function being called.  

This includes the "return address" that the CPU will go to after the function is finished.

---

### Stack Pointer

The stack pointer register contains the address of the end of the stack.  

Depending on the CPU architecture, this address may be the address to the last stored value, or the address after the last value.

When memory is allocated/released in the stack (either a variable, or an entire stack frame), the address in the stack pointer changes.  

{{% /section %}}

---

### Heap

The heap contains long-life data that persists during the lifetime of the program (rather than the lifetime of an instance of a specific function).

This memory space can be considered as the 'global scope' region, where all functions can access the data in the heap

The heap is located after the text region of the memory (where the program instructions are loaded) - i.e. near the start of the memory address space.

<!-- --- -->

<!-- ## Memory Allocation (in C) -->

<!-- #include <stdio.h>

int someValue = 0xCAFECAFE;

void doSomething() {
  doAnotherThing();
  someValue = 0xdeadbeef;
}

void doAnotherThing() {
}

int main() {
  doSomething();
  return 1;
} -->

<!-- gcc -m32 -fno-stack-protector -z execstack -Wl,-z,norelro -fno-pic -no-pie test.c -o test -->