# Approx layout of stack at the start of `main` on an `i386` machine ([source](https://www.win.tue.nl/~aeb/linux/hh/stack-layout.html)):
```
...
local variables of main
saved registers of main
return address of main
argc
argv
envp
stack from startup code
argc
argv pointers
NULL that ends argv[]
environment pointers
NULL that ends envp[]
ELF Auxiliary Table
argv strings
environment strings
program name
NULL
```
Note: First line has lowest address and last line has highest address

# Memory layout during a function call (on an `i386` machine) ([source](https://www.tenouk.com/Bufferoverflowc/Bufferoverflow2a.html))

Suppose that we have a C program:
```c
#include <stdio.h>

int MyFunc(int parameter1, char parameter2){
    int local1 = 9;
    char local2 = ‘Z’;
    return 0;
}
int main(int argc, char *argv[]){
    MyFunc(7, ‘8’);
    return 0;
}
```

Then its memory layout will look like this:

![Memory Layout](/images/memlayout_fncall.png)

# GDB:
- Setup
   1. Create a `~/.gdbinit` file with the following content: `set disassembly-flavor intel`. GDB executes this file when it starts. Feel free to add more lines.
- [Common commands](https://ccrma.stanford.edu/~jos/stkintro/Useful_commands_gdb.html)
   - `info registers eax ebx esp`: Show values stored in `eax`, `ebx`, `esp`
   - `b *0x8048544`: Set a breakpoint at the address `0x8048544`
   - `x/s 0x8048544`: Examine the string stored at address `0x8048544`
   - `x/100x $esp`: Examine memory starting at `$esp`
