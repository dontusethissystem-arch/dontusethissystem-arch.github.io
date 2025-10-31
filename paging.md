# Understanding Paging: Flat vs. Hierarchical Paging
> A simple explanation for students, system programmers and curious engineers.

## What is Paging?
Paging is a *memory management technique* that breaks physical and virtual memory into fixed-size blocks called pages.
It allow an operating system to:
* Use memory more efficiently,
* Avoid external fragmentation,
* Isolate process in their own address spaces.
Each process uses **virtual addresses**, which must be translated into physical addresses before accessing RAM.
This translation is done by the **MMU (Memory Management Unit)** using **page tables**.
--
## Flat (Single-Level) Paging
In **flat paging**, the system uses a **single page table** to map virtual pages directly to physical frames.
### Example
If a system uses 4 KB pages and 32-bit wide virtual addresses:
* There are 2^32/2^12 = 2^20 = 1,048,576 virtual pages.
* Each entry in the page table holds the physical frame number (and flags like valid, dirty, accessed).
### Problem
A single page table for each process becomes huge:
* 1,048,576 entries × 4 bytes per entry = **4 MB** per **process*.
* for 100 processes → **400 MB** just for page tables!
Thus, flat paging doesn't scale well as address spaces and processes counts grow

