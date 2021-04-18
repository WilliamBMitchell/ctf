# NM01
## BRIEFING
Retrieve output from network endpoint at `cfta-nm01.allyourbases.co` port `8017` and figure out how to get the flag.

## Solution

We start by connecting to the host and port to see what happens:

```console
root@osboxes:~/Downloads/nm01# nc cfta-nm01.allyourbases.co 8017
\x53\x4F\x4D\x4F\x4B\x57

Incorrect
```

In the above, I simply hit enter and then the output `Incorrect` was returned and the connection closed.

From observation it is apparent that we are getting bytes from the server. Through experimentation I determined that the correct response was to convert the bytes to an ASCII string. However, we have to be quick about it. If we manually convert the bytes and then submit we will recevie a `Too Slow!` message:

```console
root@osboxes:~/Downloads/nm01# nc cfta-nm01.allyourbases.co 8017
\x51\x4C\x50\x59\x54\x56
QLPYTV
Too slow!
```
In order to be fast, we can write a short script to receive the bytes, convert them to ASCII, and then send them back to the server:

```py
from pwn import * 
import binascii

HOST = 'cfta-nm01.allyourbases.co'
PORT = 8017

sh = remote(HOST,PORT)
line = sh.recvline().strip()
line = line.replace("\\x","")
line = binascii.unhexlify(line) 
sh.sendline(line)
print sh.recv()

sh.close()
```
If we run this script we get the flag:

```console
root@osboxes:~/Downloads/nm01# python nm01.py 
[+] Opening connection to cfta-nm01.allyourbases.co on port 8017: Done
Correct! - Flag: o[hex]=>i[ascii]=:)
[*] Closed connection to cfta-nm01.allyourbases.co port 8017
```

The flag is **o[hex]=>i[ascii]=:)**.
