# Understanding Paging: Flat vs. Hierarchical Paging
> A simple explanation for students, system programmers and curious engineers.

## What is Paging?
Paging is a *memory management technique* that breaks physical and virtual memory into fixed-size blocks called pages.
It allow an operating system to:
* Use memory more efficiently,
* Avoid external fragmentation,
* Isolate process in their own address spaces.
Each process uses *virtual addresses*, which must be translated into physical addresses before accessing RAM.
This translation is done by the *MMU (Memory Management Unit)* using *page tables*.

