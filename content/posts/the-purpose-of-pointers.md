---
Title: "The Purpose of Pointers"
draft: false
---

## Why Pointers Matter
So why even pointers matter in C ? What their purpose and how misusing pointers can lead to serious consequences.

C relies heavily on Pointers for managing *arrays*, *strings*, *functions* so without understanding Pointers you'll struggle to write efficient code.

### What Is a Pointer ?
to give a simple explaination, when you create a variable you hold a value, each variable stored in specific location in the Computer Memory which is called `Address`, so a Pointers is variable that stores the address of an object, it allows the program to access that object indirectly.

## The Target Code
Here is a simple example demonstrating how pointers work in C:
```c
#include <stdio.h>

int main ()
{
    int a = 15;
    int *ptr = &a;

    *ptr = 100;
    printf("ptr value: %d\n", *ptr);

    return 0;
}
```
### Under the Hood
* At first
* We create an integer containing `15`
* then:
* `int *ptr = &a;`
* We create a pointer called `ptr` and store the address of `a` inside it.
* Then:
* `*ptr = 100;`
* The `*` here means **dereference**.
* It tells C:
* **Go to the object that `ptr` points to and change its value.**
* And because `ptr` points to `a`, this:
* `*ptr = 100;`
* Is effectively changing:
* a = 100;
* So we can access `a` indirectly through the pointer.

### The serious Problems of misusing Pointers
Some serious problems can happen if you do not use pointers correctly this include:
* **Null Pointer dereference:** using a pointer that does not point to an object.
* **Dangling Pointer:** using a pointer after the object it referred to no longer exists.
* **Out-of-bounds access:** reading or writing outside an array.
* **Use-after-free:** using memory after it has been released.

These bugs can have serious consequences, depending on how the program uses the pointer and what memory is affected.
This is a reason why understanding Pointers is the first step to better understand Memory Management. 

### Seeing Things Under GDB
If we compile this program and load it inside `GDB`, we can inspect the program's memory and see the addresses involved.

First, We compile the program using the `-g` flag for debugging and we specify the name of the program if you want with `-o` flag:
```bash
gcc -g -o ptr ptr.c
```
* Next we launch GDB in quiet mode (optional) `-q`, and we run the program so we can inspect the active addresses:
* we set a break point after the `*ptr = 100`, so we can print the actual memory address and compare them, (The exact line number may be different depending on your file, in my case it's line 9):
```text
gdb -q ./ptr
(gdb) break 9
(gdb) print &a
$1 = (int *) 0x7fffffffdf84
(gdb)  print ptr
$2 = (int *) 0x7fffffffdf84
(gdb)  print print/x *ptr
$3 = 0x64
```
* Notice how `&a` and `ptr` point to the same address (0x7fffffffdf84) This shows that the pointer does not create new data, it just creates a window to an exesting memory slot.
* If your debugger shows the value in hex (`0x64`), converting it to decimal gives us exactly `100` 
* This confirms that dereferencing `ptr` changed the value of `a`.
