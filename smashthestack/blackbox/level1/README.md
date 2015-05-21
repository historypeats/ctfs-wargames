# Level 1

## Details
**Vulnerability:** Hard Coded Secret

**Memory Security:** None

**Description**

This challenge was a binary called login2 that prompts the user for the username and password. If the credentials supplied were correct, the binary would drop you in an elevated shell as the user level2. This was solved by running strings on the binary and retrieving the hard coded credentials.

## Exploit
Running strings:
```bash
level1@blackbox:~$ strings login2 
...
Username: 
Password: 
level2
PassFor2
Welcome, level 2!
/bin/sh
...
```

Getting the shell:
```bash
level1@blackbox:~$ ./login2 
Username: level2
Password: PassFor2
Welcome, level 2!
sh-3.1$ id
uid=1002(level1) gid=1005(gamers) euid=1003(level2) groups=1002(level1),1005(gamers)
```

