---
title: "Execution"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:56+10:00
draft: true
tileColour: "#8BBEE8"

---

## CPU Speed

The speed of a CPU is determined by its clock speed.  

This speed refers to the rate (in Hz) at which the processor is able to generate pulses - that are used to synchronise its operations.  

i.e. an Arduino (i.e. ATmega 328) operates at 16 MHz.  
i.e. A Ryzen 9 3900X CPU operates at 3.8 GHz.  

---

## Execution

In the CPU, there exists a Pointer Register known as the "`program counter`" (`PC`), which keeps track of the next instruction the processor should execute.  

To execute instructions the CPU performs what is known as the "_Fetch-Execute Cycle_"

---

## Fetch-Decode-Execute Cycle

* Fetch the instruction from the address in the program counter
* Increment the program counter
* Decode the instruction
* Execute the instruction

> Note: The program counter contains the **address** of the instruction  

> Note: An instruction can be multibyte  

---

## Code and Instructions

{{% section %}}

Consider the below source code

> ```c
> for (i = 0; i < 20; i++) {
>   ...
> }
> ```

This block of code requires around 4 instructions

* set `i` to `0`
* compare if `i` is less than `20`
* run the contents if true
* increment `i`

---

The following code also requires several instructions

> ```c
> if (a*2 == b + (c * 3)) {
>   ...
> }
> ```

In fact it requires around 10 instructions!

---

> 1 statement != 1 instruction 

&nbsp;  
Some instructions require multiple CPU clock cycles 

> 1 GHz clock speed !== 1 billion IPS  
> _(IPS - Instructions per Second)_

{{% /section %}}
