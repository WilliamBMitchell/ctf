## Problem ##
My teacher gave some homework in crypto. HELP!!!!!!

Connect `nc baby_homework.ichsa.ctf.today 8010`

## Solution ##

We are provided with the following source code:

```py
#!/usr/local/bin/python2

from Crypto.Cipher.AES import AESCipher
import os, random, binascii
from sys import argv
import string
import sys


def padding(data):
    pad_size = 16 - (len(data) % 16)
    data = data + "".join([random.choice(string.printable) for _ in range(pad_size)])
    return data

def encrypt(data):
    return AESCipher(os.environ.get('KEY')).encrypt(padding(data))

def main():
	print "Hello! What do you want to encrypt today?"
	sys.stdout.flush()
	user_input = raw_input()
	print binascii.hexlify(encrypt(user_input + os.environ.get('FLAG')))
	sys.exit()
	
if __name__ == '__main__':
	main()
  ```
  
After analyzing the code for a bit I noticed that the block size was 16 and that if we input 15 characters we would get back some hex that included the encoded first character of the flag in the first block. I could then run through every readable ASCII character and compare the results to the one containing the flag character. This would reveal the first character of the flag.

If I could get the first character I could them them all. I made the following script to retrieve the flag character by character:
  
```py
from pwn import *

HOST = 'baby_homework.ichsa.ctf.today'
PORT = 8010

current_val = 31
FLAG = ''

while True:
    sh = remote(HOST,PORT)
    
    payload = 'A' * current_val 
    sh.recvline()
    sh.sendline(payload)
    return_val = sh.recvline().strip()[0:64] # grabbing first 2 blocks worth of hexadecimal
                                             # the final hex character is the next letter of the flag
    sh.close()
    for i in range(33,127): # range of readable ASCII characters
        sh = remote(HOST,PORT)

        brute_force_payload = payload + FLAG + chr(i)
        sh.recvline()
        sh.sendline(brute_force_payload)
        response = sh.recvline().strip()[0:64]
        sh.close()
        if response == return_val:
            FLAG += chr(i)
            print "Flag: ", FLAG
            current_val -= 1
            break
```

Running the script we retrieve the flag:

```console
[+] Opening connection to baby_homework.ichsa.ctf.today on port 8010: Done
[*] Closed connection to baby_homework.ichsa.ctf.today port 8010
[+] Opening connection to baby_homework.ichsa.ctf.today on port 8010: Done
[*] Closed connection to baby_homework.ichsa.ctf.today port 8010
[+] Opening connection to baby_homework.ichsa.ctf.today on port 8010: Done
[*] Closed connection to baby_homework.ichsa.ctf.today port 8010
Flag:  d
```

It takes a couple minutes but eventually the full flag comes out:

```console
[+] Opening connection to baby_homework.ichsa.ctf.today on port 8010: Done
[*] Closed connection to baby_homework.ichsa.ctf.today port 8010
[+] Opening connection to baby_homework.ichsa.ctf.today on port 8010: Done
[*] Closed connection to baby_homework.ichsa.ctf.today port 8010
[+] Opening connection to baby_homework.ichsa.ctf.today on port 8010: Done
[*] Closed connection to baby_homework.ichsa.ctf.today port 8010
Flag:  d0n7_7ruzt_DeF4uL7_V4lu3z
```

The flag is **ICHSA_CTF{d0n7_7ruzt_DeF4uL7_V4lu3z}**.
