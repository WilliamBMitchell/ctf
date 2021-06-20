## Problem ##

hmm, fibonacci is fun, right?

`nc extended-fibonacci-sequence.hsc.tf 1337`

P.S: This chall was made by an anonymous CS Club member :)

## Solution ##

See [Extended Fibonacci Sequence New](Extended+Fibonacci+Sequence+New.pdf) for the problem description:

I took the approach of storing the results for all possible values of `n` in a file and then reading them from the file during interaction with the server. I pulled a speedy method of calculating fibonacci numbers using linear algebra from [here](https://stackoverflow.com/questions/18172257/efficient-calculation-of-fibonacci-series).

Here is my solution:

```py
from pwn import *

# https://stackoverflow.com/questions/18172257/efficient-calculation-of-fibonacci-series
def fib(n):
    v1, v2, v3 = 1, 1, 0    # initialise a matrix [[1,1],[1,0]]
    for rec in bin(n)[3:]:  # perform fast exponentiation of the matrix (quickly raise it to the nth power)
        calc = v2*v2
        v1, v2, v3 = v1*v1+calc, (v1+v3)*v2, calc+v3*v3
        if rec=='1':    v1, v2, v3 = v1+v2, v1, v2
    return v2

fibonacci_numbers = []
for i in range(1001):
    fibonacci_numbers.append(fib(i))

fibonacci_numbers[0] = 0

S_n = []
current_S_n = ''
for j in range(len(fibonacci_numbers)):
    val = str(fibonacci_numbers[j])
    if 'L' in val:
        val = val[:-1]
    current_S_n += val
    S_n.append(current_S_n)

# ALL RESPONSES STORED IN extended-fib-responses.txt
'''
total = 0
responses = ''
for k in range(len(fibonacci_numbers)):
    val = S_n[k]
    if k != 0:
        val = val.lstrip("0")
    val = int(val)
    total += val
    
    result = str(total)
    if len(result) > 11:
        result = result[-11:]
    responses += str(result)
    responses += " "

with open('extended-fib-responses.txt','w') as f:
    f.write(responses)
'''
f = open('extended-fib-responses.txt')
line = f.readlines()[0]
responses = line.split()

HOST = 'extended-fibonacci-sequence.hsc.tf' 
PORT = 1337

sh = remote(HOST,PORT)
sh.recvline() # == proof-of-work: disabled ==
for i in range(100):
    n = sh.recvline()
    n = int(n.strip())
    print "[+] n = ", str(n)
    response = responses[n]
    print "[+] Response: ",response
    sh.recvuntil(": ")
    sh.sendline(response)
```

Here is the output:

```console
root@osboxes:~/Downloads# python extended-fibonacci.py 
[+] Opening connection to extended-fibonacci-sequence.hsc.tf on port 1337: Done
[+] n =  275
[+] Response:  97056610376
[+] n =  588
[+] Response:  54110266664
[+] n =  849
[+] Response:  62975748568
[+] n =  606
[+] Response:  20906217140
[+] n =  641
[+] Response:  37439278756
[+] n =  893
[+] Response:  32975112524
[+] n =  162
[+] Response:  17988556092
[+] n =  991
[+] Response:  35732215057
[+] n =  3
[+] Response:  124
[+] n =  900
[+] Response:  88182084120
[+] n =  456
[+] Response:  24028301248
[+] n =  166
[+] Response:  49047141135
[+] n =  388
[+] Response:  63809712439
[+] n =  800
[+] Response:  78115138070
[+] n =  902
[+] Response:  17169761722
[+] n =  375
[+] Response:  96267516476
[+] n =  685
[+] Response:  63358866577
[+] n =  635
[+] Response:  78685905336
[+] n =  528
[+] Response:  22515157584
[+] n =  705
[+] Response:  28323698232
Traceback (most recent call last):
  File "extended-fibonacci.py", line 63, in <module>
    n = int(n.strip())
ValueError: invalid literal for int() with base 10: "b'flag{nacco_ordinary_fib}'"
[*] Closed connection to extended-fibonacci-sequence.hsc.tf port 1337
```

The flag is **flag{nacco_ordinary_fib}**.
