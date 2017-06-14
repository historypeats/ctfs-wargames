# BOF

Looking at the source code we know there's a classic buffer overflow in the func function. Also, the binary is compiled with stack canaries, so if we overwrite the canary, and return from the function, we will get a stack smashing error and the binary will crash. Luckily for us we just need to overwrite the key parameter passed in on the stack and we get shell access.

### exploit
```bash
(python -c 'print "A" * 52 + "\xbe\xba\xfe\xca"' && cat) | nc pwnable.kr 9000
```
Note: I was oddly stuck trying ti submit the exploit. It worked locally in radare2, but not remotely. Turns out I needed some stupid trick with "cat" to be able to submit the exploit code after scanf() runs. Without cat, the execution would end before scanf() was reached.

flag: daddy, I just pwned a buFFer :)
