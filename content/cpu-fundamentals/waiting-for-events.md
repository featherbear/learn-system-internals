---
title: "Waiting for Events"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:57+10:00
tileColour: "#8BBEE8"

---

{{< slide class="center" >}}

# Waiting for Events

---

## Waiting for Events

If a system requests for a resource, it will need to wait for that resource to become available before it can continue.

There are two methods to wait for resource availability - synchronous waiting (polling) and asynchronous waiting (interrupts)

---

## Polling

Every `x` time interval, the software will query the I/O device to see if it available.  
Whilst no extra hardware is required (polling is software-based), it is not efficient due to wasted time checking for the I/O.

---

## Interrupts

A _Programmable Interrupt Controller_ (`PIC`) inside the CPU listens for signals from the I/O devices, and causes the CPU to pause its operation and handle the 'interrupt event'.  

The PIC works alongside the rest of the CPU to perform the following three tasks

* Recognise interrupt events
* Respond to the interrupt
* Resume the previous task

---

## Interrupts

When an _Interrupt Request_ (`IRQ`) is received by the PIC, the _Interrupt Vector Table_ (`IVT`) is accessed to find the relevant _Interrupt Service Routine_ (`ISR`) to execute.  

The ISR is the function that is executed which handles the interrupt

---

## Interrupt Delay

When an interrupt is received, it takes a little while to transfer CPU execution control to the ISR. This delay is known as the `interrupt latency`, and is roughly 5 clock cycles - to save the return address, registers, and to jump to the ISR

---

## Interrupt Priority

_If multiple interrupts are received at the same time, which one is handled first???_

* Daisy-Chain Priority Resolution
  * The first I/O device that receives the _Interrupt Acknowledge_ (`INTA`) signal from the CPU will only pass the `INTA` signal to the next device when it has finished.

* Separate IRQ bus lines
  * `IRQ0` > `IRQ1` > `IRQ2` > `...`

---

## Interrupt Control

It is favourable to prevent other interrupts from being executed until the previous ISR is complete. There are two methods to achieve this.

* Interrupt Masking
  * Interrupt mask in the control register
  * Some interrupts are non-maskable
    * i.e. important interrupts for power failures
* Global Interrupt Enable Bit
  * Flag in the control register
