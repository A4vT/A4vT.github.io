---
Title: "The Stack And The Heap"
draft: true
tags:
- Memory Management
---

## What is The Stack and Heap Memory and Why do they exist ?
* To give a simple Explanation, imagine the Stack is like a neat pile of papers on your desk, stores temporary things your program is actively using and automatically disappearing when no longer needed.Since Memory is managed by **The System** it is Fast and Efficient but has limited space to Heap Allocation.
* The Heap on the other hand, is like a large storage room, It Hold bigger, Longer-Lasting items Your program requests but you must Free Memory **Manually** when finished.
* Heap considered Less safe than the Stack because Heap data is Accessible by Multiple threads increasing the risk of Data Corruption and Memory Leaks if not Handled properly.
* So to answer to the question *Why do they exist?* Simply because computer needs two ways to remember things:
 - It needs the Stack for Speed to handles Short-terms memory that computer can instantly throw away the second a task finished.
 - And it needs the Heap To Handle complex, long-terms unpredictable Memories.

## Common Memory corruption
- Memory Leak
- Stack buffer overflow
- Double-free
- Heap overflow

## Common Memory Bugs

## Why this matters for Exploit Development

## under the hood
run code + gdb

## What i Learned
