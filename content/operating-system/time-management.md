---
title: "Time Management"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:43+10:00
draft: true
tileColour: "#D7A9E3"
---

<!-- https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/ -->

A processor can only run one thing at a time, including the OS and the Kernel.

A processor can only run one thing at a time. This include the OS itself.
When an application is running, the operating system itself is paused. However this does not feel apparent as regular interrupts (i.e. timers, interrupts) give control back to the OS.

---

---

|Single Program|Multiple Programs|
|:------------:|:---------------:|
|![](https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/Screenshot%20from%202020-02-19%2017-30-08.png)|![](https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/Screenshot%20from%202020-02-19%2017-30-15.png)|

---

---

![](https://featherbear.cc/UNSW-COMP3231/post/processes-and-threads/Screenshot%20from%202020-02-19%2017-30-21.png)

### Process State

A process can be in one of the following states

* **RUNNING** - Process is currently active
* **READY** - Process is inactive, but ready to run
* **BLOCKED** - Process is inactive, but not ready to run

Running->Ready happens on a yield / end of time slice
Running->Blocked happens when a process is waiting for input (file, network), timer (alarm), resource

Scheduler / Dispatcher

Efficiently decides which process should run next (Queue)
A queue is also used to decide the next blocked process to check.
Event types (and consequently their respective event queue) can be created dynamically.

--

https://featherbear.cc/UNSW-COMP3231/post/thread-model/

Kernel Threads, User Threads

Kernel entry delay