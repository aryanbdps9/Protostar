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

## Calling convention in 64 bit x86 machines
Everything will be same, except first 6 args will be passed through registers in the following order: RDI, RSI, RDX,RCX, R8D, R9D. So, the args of a function will look like: f(RDI, RSI, RDX,RCX, R8D, R9D, RBP). Other args go on stack as usual.

# GDB:
- Setup
   1. Create a `~/.gdbinit` file with the following content: `set disassembly-flavor intel`. GDB executes this file when it starts. Feel free to add more lines.
- [Common commands](https://ccrma.stanford.edu/~jos/stkintro/Useful_commands_gdb.html)
   - [Hooks](https://sourceware.org/gdb/onlinedocs/gdb/Hooks.html)
   - [`command`](https://youtu.be/ZHghwsTRyzQ?t=290)
   - `info registers eax ebx esp`: Show values stored in `eax`, `ebx`, `esp`
   - `b *0x8048544`: Set a breakpoint at the address `0x8048544`
   - `x/s 0x8048544`: Examine the string stored at address `0x8048544`
   - `x/100x $esp`: Examine memory starting at `$esp`
   - `x/3i 0x80483cc`: Examine three instructions starting at memory location `0x80483cc`
   - `backtrace`: Traces the sequence of function calls which lead to current state in execution. Warning: It won't work well if you've smashed the return pointer of any function.
   - [Set type of variables](https://youtu.be/TfJrU95q1J4?t=538)

# Misc.
- Format of format strings: `%[argument_index$][flags][width][.precision]conversion`
