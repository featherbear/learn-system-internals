---
title: "The Operating System"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:14:08+10:00
tileColour: "#D7A9E3"
---

{{< slide class="center" >}}

# Operating Systems

---

{{% section %}}

## The Role of the
## Operating System

An Operating System can be seen in two ways

* As a layer of abstraction (_"Abstract Machine"_)
* As a resource manager

---

### Abstract Machine

The operating system extends the basic hardware set, adding new functionality that is usable by programs.

The OS hides the specifics and details of the hardware, and instead provides a unified interface to the system.

### Resource Manager

The operating system fairly allocates resources (memory, execution time, etc) to users and processes. It ensures that the system continues to execute.

---

### Why?

In order for applications to be interoperable and run concurrently, there needs to be a common framework for applications to use - That is the Operating System

{{% /section %}}

---

## Inside An Operating System

{{% section %}}

An operating system is comprised of a **kernel** and a set of supporting software.

* OS: Windows 10 - Kernel: Windows NT  
* OS: Mac OS Catalina - Kernel: XNU  
* OS: Ubuntu - Kernel: Linux

---

### Kernel

The Kernel is the core part of the operating system.  
It is responsible for all of the fundamental operations of the operating system.  

For example...

* Deciding which application should next run
* Allocating memory to an application
* Checking if a user has access to a file

---

> Applications run on the OS

> The OS runs on the Kernel  

{{% /section %}}
