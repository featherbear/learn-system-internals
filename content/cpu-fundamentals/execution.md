---
title: "Execution"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:56+10:00
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

---

## Branching

{{% section %}}

In machine/assembly code, there is no such keyword atom for an 'if statement'.  

Instead, this is achieved by performing an instruction, followed by another instruction that uses the results of the first instruction.

---

i.e. Possible Intel x86 assembly code for  
`if (x == 3) { ... }`

```asm
mov eax, [ebp-0x8]
mov ebx, 3
cmp eax, ebx   <--- This instruction compares x with 3
je 0x11223344  <--- The results of the compare is used
```

When the `cmp` instruction is executed, a flag in the [status register](../overview/#/5) of the CPU is set to reflect `x - 3 == 0`.  
When the `je` instruction is executed, this flag is checked to see if it is true.

---

> `je 0x11223344`

This is the "jump if equal" Intel x86 instruction.  

When executed, the CPU will look inside its Status Register, and check if the Zero Flag (`ZF`) is set.

If set, the CPU will set its program counter (`PC`) to `0x11223344`. During the next _Fetch-Execute Cycle_, the instructions at `0x11223344` will be executed.

This process is known as ***branching***.

---

### Branch Delay Slots

When a branching instruction has been executed, the CPU does not immediately execute the new instruction (as a result of the Fetch-Execute cycle).  

Instead of 'doing nothing' for those CPU cycles, an additional instruction can be executed before the CPU begins to execute the new address instructions.

This additional instruction is known as a "branch delay slot instruction".  

---

In cases where no additional instruction is wanted, a `nop` (no-operation) instruction can be placed in the branch delay slot.

```mips
li  $a0, 42
jal fun     <--- Instruction to jump to the label at `fun`
nop         <--- Branch delay slot - do nothing
sw  $v0, x  <!-- Will not execute as program has branched
```

{{% /section %}}

---

## Functions

{{% section %}}

When a function is called, the processor will allocate some memory on the stack for the new function's stack frame. Inside the stack frame, it will store data crucial to the continuation of the system.

These include the current stack and base pointer addresses, and the address of the next instruction (which will be executed when the function returns)

---

Compared to the RAM in a computer system, there are a very limited number and capacity of registers in the CPU. When a function is called, it will most likely require the modification of these registers.

However if a function `A` relies on the execution of function `B`, but both functions require the use of registers - we will run into issues!

As consequence, we need to deal with these so called _conflict registers_

---

### Conflict Registers

A conflict register is a term given to a register which is modified in a function.  

As the register will be modified, it is a good idea to store the original value of the register, so that it can be restored after the function finishes.  

These operations are performed in the function epilogue and function prologue.

---

### Prologue

> aka "setting up the stack frame"

The function prologue is a set of assembly instructions that are executed before the actual instructions of the function is called.

In the prologue, the conflict registers are stored on the stack - often with the `push` instruction

---

### Epilogue

> aka "tearing down the stack frame"

The function epilogue is a set of assembly instructions executed after the function instructions are called.

In the epilogue, the original registered are restored from the stack - often with the `pop` instruction. These registers should be `pop`'d in the reverse order that they were `push`'d

---

### Example Prologue and Epilogue

```
push r1       ; Function prologue
push r2       ;   Store the conflict registers r1, r2, r3
push r3

mov r1, 0xEF  ; your assembly instructions here
addi r2, 13
clr r3
xor r1, r3

pop r3        ; Function epilogue
pop r2        ;   Restore the conflict registers r3, r2, r1
pop r1
leave
ret
```

{{% /section %}}