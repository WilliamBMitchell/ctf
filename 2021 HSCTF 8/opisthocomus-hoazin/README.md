## Problem ##

The plural of calculus is calculi.

## Solution ##

We inspect the provided files. 

[opisthocomus-hoazin.py](opisthocomus-hoazin.py):

```py
import time
from Crypto.Util.number import *
flag = open('flag.txt','r').read()
p = getPrime(1024)
q = getPrime(1024)
e = 2**16+1
n=p*q
ct=[]
for ch in flag:
    ct.append((ord(ch)^e)%n)
print(n)
print(e)
print(ct)
```

[output.txt](opisthocomos-output.txt):

```console
15888457769674642859708800597310299725338251830976423740469342107745469667544014118426981955901595652146093596535042454720088489883832573612094938281276141337632202496209218136026441342435018861975571842724577501821204305185018320446993699281538507826943542962060000957702417455609633977888711896513101590291125131953317446916178315755142103529251195112400643488422928729091341969985567240235775120515891920824933965514217511971572242643456664322913133669621953247121022723513660621629349743664178128863766441389213302642916070154272811871674136669061719947615578346412919910075334517952880722801011983182804339339643
65537
[65639, 65645, 65632, 65638, 65658, 65653, 65609, 65584, 65650, 65630, 65640, 65634, 65586, 65630, 65634, 65651, 65586, 65589, 65644, 65630, 65640, 65588, 65630, 65618, 65646, 65630, 65607, 65651, 65646, 65627, 65586, 65647, 65630, 65640, 65571, 65612, 65630, 65649, 65651, 65586, 65653, 65621, 65656, 65630, 65618, 65652, 65651, 65636, 65630, 65640, 65621, 65574, 65650, 65630, 65589, 65634, 65653, 65652, 65632, 65584, 65645, 65656, 65630, 65635, 65586, 65647, 65605, 65640, 65647, 65606, 65630, 65644, 65624, 65630, 65588, 65649, 65585, 65614, 65647, 65660]
```

It took me a minute, but based on the content of `output.txt` we can surmise that something didn't go quite right during the RSA computation. Specifically, all the ciphertext values appear to be fairly close to the `e` value of `65537`. 

Eventually, I hit on it, in the `opisthocomus-hoazin.py` code, specifically this part:
```console
for ch in flag:
    ct.append((ord(ch)^e)%n)
```
We note that the `^` is the XOR operator in Python. So instead of taking a number to an exponent (`**` operator in Python), we are XORing the number with `e` or `65537`.

This revelation turns this from an RSA problem into an XOR problem. I wrote the following short script to uncover the flag:

```py
import string

arr = [65639, 65645, 65632, 65638, 65658, 65653, 65609, 65584, 65650, 65630, 65640, 65634, 65586, 65630, 65634, 65651, 65586, 65589, 65644, 65630, 65640, 65588, 65630, 65618, 65646, 65630, 65607, 65651, 65646, 65627, 65586, 65647, 65630, 65640, 65571, 65612, 65630, 65649, 65651, 65586, 65653, 65621, 65656, 65630, 65618, 65652, 65651, 65636, 65630, 65640, 65621, 65574, 65650, 65630, 65589, 65634, 65653, 65652, 65632, 65584, 65645, 65656, 65630, 65635, 65586, 65647, 65605, 65640, 65647, 65606, 65630, 65644, 65624, 65630, 65588, 65649, 65585, 65614, 65647, 65660]

e = 2**16+1 # 65537
flag = ''
alphabet = string.printable

for val in arr:
    for letter in alphabet:
        if val == (ord(letter) ^ e): # XOR, not power!
            flag += letter
            break

print flag
```

Running the script immediately yields the flag:

```console
root@osboxes:~/Documents/hsctf2021# python opisth-solve.py 
flag{tH1s_ic3_cr34m_i5_So_FroZ3n_i"M_pr3tTy_Sure_iT's_4ctua1ly_b3nDinG_mY_5p0On}
```
