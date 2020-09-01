---
title: "Compilers"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:55+10:00
tileColour: "#8BBEE8"
---

{{< slide class="center" >}}

# Compilers 

---

## The Language of the CPU

{{% section %}}

As with all digital circuitry logic - the CPU only understands binary instructions.  

We call these instructions "machine code".

---

Machine code instructions are specific to the CPU architecture (Intel x86, amd64, MIPS, AVR, etc...) and will only work for its intended architecture.

Modern day programming languages often have features like variables, macros and switch statements.

Whilst easy to understand by us humans, the computer itself won't understand these things - which is why we have compilers.

{{% /section %}}

---

## Compilers

{{% section %}}

The function of a compiler is to read in high level program source code, and output a binary file containing machine code for the required architecture.

A compiler goes through the following stages in order

* Lexical Analysis
* Syntax Analysis
* Semantic Analysis
* Optimisation
* Code Generation

<!-- https://www.tutorialspoint.com/compiler_design/compiler_design_phases_of_compiler.htm -->

---

### Lexical Analysis

The compiler reads the program source code and represents the parts of the code as tokens

---

### Syntax Analysis

The compiler analyses the tokens and forms a syntax tree (parsing tree).  
Syntactical correctness is performed (i.e. valid syntax)

---

### Semantic Analysis

The compiler analyses the syntax tree to ensure that the tree nodes follow the rules of the language

---

### Optimisation

> Optional

The compiler optimises the syntax tree by rearranging, removing or even adding code in the syntax tree

---

### Code Generation

The compiler maps each node in the syntax tree to the machine code of the target architecture

{{% /section %}}

---

## Just-In-Time (JIT) Compilers

Some compilers (i.e. Java) do not compile the program into machine code, but into what is known as "`bytecode`".  

When the program is run, an interpreter running on the system will map the bytecode instructions to the machine instructions native to that system.  

This therefore makes the bytecode-compiled binary portable and universal, without needing to compile the program specifically for each system.

---

## Assembly Code

{{% section %}}

Assembly code is a representation of the machine-code, used to bring clarity and understanding of the machine-code to developers.

Left: Machine-code (i.e. `\xf3\x0f\x1e\xfb`)  
Right: Assembly code representation (i.e. `nop`)

![](2020-09-01_20-07-26.png)

---

We are also able to write programs in assembly language ourselves!  

{{% /section %}}
