# Level 4

## Details
**Vulnerability:** Directory Traversal

**Memory Security:** None

**Description**

This challenge was a binary called "shared" that returns the contents of a file. It is a basic directory traversal vunerability, which should be exploited to read the password of level5's password file. At first glance, it looks like there is code to prevent the use of "/../" to traverse up directories, however, it is possible to circumvent the filter. The trick is that it strips the character sequence "/../". Therefore, we can bypass this filter by embedding "/../" inside another "/../". For example, "/./.././". When the code strips "/../" from the middle of the sequence, the "/." and "./" will collapse together forming "/../". 

Vulnerable Code:
```c
relative_path = strreplace(ptr, "/../", "");
relative_path = strreplace(relative_path.c_str(), "/./", "");
```

## Exploit
Create a sequence of "/./.././" that will allow us to traverse to level5's password file:
```bash
level4@blackbox:~$ ./shared lyrics/./../././../././../././.././home/level5/password
Contents of /usr/share/level5/lyrics/../../../../home/level5/password:
Traveller
```

