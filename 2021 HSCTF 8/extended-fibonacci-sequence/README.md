## Problem ##

hmm, fibonacci is fun, right?

`nc extended-fibonacci-sequence.hsc.tf 1337`

P.S: This chall was made by an anonymous CS Club member :)

## Solution ##

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
