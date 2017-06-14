# fd
We want to read from STDIN, which the file descriptor is 1. Looking at the C code, we see it takes user input and substracts 0x1234, which is 4660. So we just need to do the math, and make sure the outcome of the subtraction is 1.

```
fd@ubuntu:~$ ./fd 4661
Then enter "LETMEWIN"
```

flag: mommy! I think I know what a file descriptor is!!
