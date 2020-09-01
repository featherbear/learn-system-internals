---
title: "The CPU"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:12:54+10:00
tileColour: "#8BBEE8"

---

## What's in a CPU?

{{% section %}}

Inside a CPU is a group of different microchips that collectively work together to serve the functionality of a CPU.

---

![](https://upload.wikimedia.org/wikipedia/commons/d/d8/ABasicComputer.gif)

{{% /section %}}

---

## I/O

To interact with other devices on the system, the CPU contains input and output lanes which are physically connected (via 'busses' on the circuit board) to those devices.

I/O can include keyboards, mice, monitors, speakers.

---

## Registers

Registers are data storages that lie in the CPU. They have a fixed size (as specified by the CPU architecture).  

Compared to other memory devices (i.e. RAM, SSDs, HDDs, etc...) - the CPU's access to its registers is very fast.

The CPU uses registers to perform all of its state-related operations.

---

## General Purpose Registers

{{% section %}}

The function and purpose of General Purpose registers are dictated by the software and code being executed.  

When data from the RAM / other storage devices are requested, they are eventually loaded into these registers to be processed. 

Programs will generally interact with these registers the most.

---

_i.e. Intel x86 Architecture: 8, 16, 32 bit registers_

<img src="https://www.cs.virginia.edu/~evans/cs216/guides/x86-registers.png" width="70%" />

{{% /section %}}

---

## Location Registers

Location registers contain various addressed required by the CPU to perform instructions.

* The Pointer Register contains the current instruction address
* The Stack Pointer Register contains the current location of the stack
* The Base Pointer Register contains the address of the current stack frame

More on this later...

---

## Flag Registers

A CPU would contain a flag register - that holds various attributes about the CPU's current state. These are used by future instructions

* e.g. `30-30` => `zero flag` = `1`
* e.g. `30-15` => `zero flag` = `0`
* e.g. `30-50` => `sign flag` = `1`

Each different flag is (usually) 1-bit long, and such a 32-bit flag register can hold 32 different attributes.

---

## Control Registers

Control registers dictate the deep internal operation of how the CPU uses its registers, resources and other devices in the system