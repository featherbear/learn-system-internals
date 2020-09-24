---
title: "Time Management"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:43+10:00
tileColour: "#D7A9E3"
---

{{< slide class="center" >}}

## Time Management

---

## Access Management

<!-- https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/ -->

{{% section %}}
A processor can only run one thing at a time.  
This include the OS itself.

> Application is running - OS paused  
OS running - application paused

However this does not feel apparent (perceived performance) as regular interrupts (i.e. timers, interrupts) cause the processor to continually switch between applications and the OS.

---

If the CPU did not switch between the different applications, future applications must wait for the termination of previous tasks.

(Computer tasks appear sluggish)

![](https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/Screenshot%20from%202020-02-19%2017-30-08.png)

{{% /section %}}

---

{{< slide class="center" >}}


|Program State|Execution Graph|
|:------------:|:---------------:|
|![](https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/Screenshot%20from%202020-02-19%2017-30-15.png)|![](https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/Screenshot%20from%202020-02-19%2017-30-21.png)|

Inside each [process](../processes-and-threads/#/2) / [thread](../processes-and-threads/#/5) control block, the current state of the process/thread is stored - allowing the CPU to perform a _context switch_

---

## Context Switches

{{% section %}}
A **context switch** is the event that occurs when the CPU changes from one process/thread to another. When a context switch occurs, the state of the current task is saved, and the next task's state is restored.  

These states include registers, program counters, file information, memory addresses, [etc...](../processes-and-threads/#/2)

---

Processes and threads are oblivious to the occurrence of a context switch. When their state is restored, their execution continues as if nothing had happened.

A context switch can occur programmatically (i.e. a blocking syscall, the `exit()` function), as well as automatically (exceptions and interrupts). They can also occur between any two CPU instructions

<!-- TODO: Locks? -->

{{% /section %}}

---

## Scheduling

To co-ordinate which task should run, and for how long (time slice), an Operating System implements a _scheduler_.

A simpler scheduler may designate each task to run for X cycles before switching to the next task.

More advanced schedulers may implement a mixture of priority, bound-ness and queuing algorithms to decide the next best task

---

## Process Run State

{{% section %}}
A process can be in one of the following states

* **RUNNING** - Process is currently active
* **READY** - Process is inactive, but ready to run
* **BLOCKED** - Process is inactive, but not ready to run

&nbsp;  

`Running->Ready` happens on a yield / time slice.  
`Running->Blocked` happens when a process is waiting for a resource

{{% note %}}
input (file, network), timer (alarm), resource
{{% /note %}}

---

Modern operating systems implement task queues to sort which tasks are ready to execute

When a running task reaches the end of its time slice / or it programmatically yields its execution. The scheduler will pick the next task to execute from the "Ready Queue". 

Tasks in the "Blocked Queue" are asynchronously moved to the "Ready Queue" when their prior resource request is fulfilled

{{% /section %}}

---

## Task Priority

{{% section %}}

### Priority

A task with a higher priority will have a higher probability of being chosen to execute by the scheduler

---

### CPU/IO Bound-ness

A CPU-bound task spends most of its time in computation, whose completion is dependent on the time allocated.

An IO-bound process spends most of its time waiting for an IO event.

---

### CPU/IO Bound-ness

Executing a CPU-bound task greatly affects the execution of an IO-bound task, as the IO operation is blocked until the CPU-bound task is completed.

Schedulers aim to <u>prioritise IO-bound tasks</u>, as they can quickly execute and yield if still waiting
{{% /section %}}

---

## The Cost of a Context Switch

{{% section %}}
When a context switch occurs it must

* Switch to the Operating System code
* Exit user mode, enter kernel mode
* Store the current state (PC, registers, stack, etc...)
* Find the next task to run (scheduler)
* Restore the task state
* Exit kernel mode, enter user mode
* Execute the application instruction

> This takes time!

---

It is important for the scheduler to perform context switches frequently, such that all processes and threads have time to execute and complete.

However the execution time slice cannot be too short, else the CPU will spend more time context switching than actually executing the task!

{{% /section %}}
