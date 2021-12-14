## Solution ##

```py
from pwn import *
import commands
import re
import codecs
import base64

HOST = 'crypto.chal.csaw.io'
PORT = 5001

sh = remote(HOST, PORT)

morse_numbers = {'-----': '0', '.----':'1', '..---':'2', '...--':'3', '....-':'4', '.....':'5', '-....':'6', '--...':'7', '---..': '8', '----.': '9'}
while(True):
    try:
        sh.recvuntil("What does this mean?")
        sh.recvline() # Blank line
        sample =  sh.recvline().rstrip()
            
        sample = sample.split('/')

        decimal = []

        for morse in sample:
            morse = morse.split()
            num = ''
            for item in morse:
                num += morse_numbers[item]
            decimal.append(num)

        base64s = ''.join([chr(int(i)) for i in decimal])

        rsa_info = base64.b64decode(base64s)

        rsa_info = rsa_info.split('\n')

        N = rsa_info[0].split()[-1]
        e = rsa_info[1].split()[-1]
        c = rsa_info[2].split()[-1]

        rsa_result =  commands.getstatusoutput("python3 /opt/RsaCtfTool/RsaCtfTool.py -n %s -e %s --uncipher %s --attack cube_root" % (N, e, c))

        rot13_text =  re.findall("'([^']*)'", rsa_result[1].split('STR')[1])[0]
        secret = codecs.encode(rot13_text, 'rot_13')

        print secret
        sh.sendline(secret)
    except:
        print sh.recv()
```
