---
title: "Understanding Paging: Flat vs. Hierarchical"
author: "Malinga RK"
date: "2025-10-30"
---
# Understanding Paging: Flat vs. Hierarchical Paging
> A simple explanation for students, system programmers and curious engineers.
---
## What is Paging?
Paging is a **memory management technique** that breaks physical and virtual memory into fixed-size blocks called pages.
It allow an operating system to:
- Use memory more efficiently,
- Avoid external fragmentation,
- Isolate process in their own address spaces.
Each process uses **virtual addresses**, which must be translated into physical addresses before accessing RAM.
This translation is done by the **MMU (Memory Management Unit)** using **page tables**.
---
## Flat (Single-Level) Paging
In **flat paging**, the system uses a **single page table** to map virtual pages directly to physical frames.
### Example
If a system uses 4 KB pages and 32-bit wide virtual addresses:
- There are $2^{32}/2^{12}$ = $2^{20}$ = 1,048,576 virtual pages.
- Each entry in the page table holds the physical frame number (and flags like valid, dirty, accessed).
### Problem
A single page table for each process becomes huge:
- 1,048,576 entries × 4 bytes per entry = **4 MB** per **process**.
- for 100 processes → **400 MB** just for page tables!
Thus, flat paging doesn't scale well as address spaces and processes counts grow
---

Hierarchical (Multi-Level) Paging

To solve the memory overhead problem, modern systems use **hierarchical paging** -- breaking the page table into smaller, manageable parts that can be loaded on demand.
### Example (Typical 2-level paging for 32-bit systems):
The virtual address is split into parts:
| Field | Bits | Description |
|------|----|-------------|
| Dir   | 10   | Page Directory Index |
| Tbl   | 10   | Page Table Index |
| Offset| 12   | Offset within Page |

- **Page Directory (Dir)** points to the **Page Tables (Tbl)**, each mapping $1,204$ pages.
- Only the portions of the table that are needed are kept in memory.

### Advantages:
* Saves memory --- page table are created only when needed.
* Easier to manage large address spaces.
* Supports demand paging and hierarchical caching (TLBs).

### Disadvantages:
* More lookups per process --- increases translation time.
* Needs hardware support (MMU + TLB caching).

# Real-world Example: x86-64 (4-level paging)
Modern 64-bit CPUs (like Intel and AMD) use **$4$** or **$5$ levels** of paging (typically 48-bit VA).
| PML4 | PDPT | PD | PT | Offset |
|------|-------|-------|-------|------|
|9 bits| 9 bits | 9 bits | 9 bits | 12 bits|
* Each level indexes 9 bits of the virtual address.
* Translation can take multiple memory accesses -- but is heavily optimized by the **TLB (Translation Lookaside Buffer)**.
---
## 🧭 Summary

| Feature | Flat Paging | Hierarchical Paging |
|----------|--------------|--------------------|
| **Structure** | One big page table | Multi-level tables |
| **Memory Use** | Very high | Much lower |
| **Address Translation** | Single lookup | Multi-step (2–5 levels) |
| **Performance** | Fast (in theory) | Slower per access, but TLB-cached |
| **Used In** | Small/simple systems | Modern CPUs (x86, ARM, RISC-V) |

---
>  **Key Takeaway:**  
> Flat paging is conceptually simple but memory-hungry.  
> Hierarchical paging is more complex but scalable — it’s the foundation of all modern virtual memory systems.

###  Further Reading

- *Operating Systems: Three Easy Pieces* — Chapter on Virtual Memory  
- Intel® SDM Vol. 3A, Chapter 4 (Paging)  
- [Paging on x86_64 — OSDev Wiki](https://wiki.osdev.org/Paging)