## Problem ##

How can we possibly decode this? (Please note that [:-1] does not remove anything from the flag, as it just strips the newline character)

## Solution ##

I thought this was an interesting problem It is reliant on the `seed` function from Python's `random` package. The idea is that if you use the same seed you can get the same "random" output.

We can take a look at `shuffle.py`:

```py
#!/usr/bin/python3
import random

f = open("flag.txt", "r").read()
random.seed(random.randint(0, 1000))
l = list(f[:-1])
random.shuffle(l)
print(''.join(l))
```

This seems simple enough. We can see that the seed used was randomly selected from 0 to 1000. This random seed produced the following shuffle of the flag:

`zftr}__g5y_ee0y1{graua00n1l`

In order to solve this problem, I relied on knowledge of the flag format, that is, the flag would begin with `flag{` and end with `}`. This would be enough information to solve the problem.

I created a dummy flag that also began with `flag{` and ended with `}`. The content betweeen these doesn't really matter, as long as the length of the dummy flag is the same as the length of the shuffled flag. I filled my dummy flag with capital letters:

`flag{ABCDEFGHIJKLMNOPQRSTU}`

Now, the strategy I used to obtain the flag was to shuffle the dummy flag for all seed values between 0 and 1000. I then compared each result to the shuffled flag and identified a few key characters to hone in on. Specifically, I used the locations of `f`, `l`, and `}` to test each seed.

Only one seed value came back positive for the locations of `f`, `l`, and `}`. 

Knowing the seed, I recreated the flag based on the transposed values from the dummy flag to the shuffled version of the dummy flag according to the seed that came back positive.

I wrote the following script to expedite the process of reversing the flag:

```py
#!/usr/bin/python3
import random

shuffled = 'zftr}__g5y_ee0y1{graua00n1l'
dummy = 'flag{ABCDEFGHIJKLMNOPQRSTU}'

for i in range(1000):
    tmp = 'flag{ABCDEFGHIJKLMNOPQRSTU}'
    random.seed(i)
    tmp = list(tmp)
    random.shuffle(tmp)
    if tmp[1] == 'f' and tmp[4] == '}' and tmp[-1] == 'l':
        print(i, " [+] found the seed!")
        print("Dummy: ", dummy)
        print("tmp: ", ''.join(tmp))
        print("Shuffled: ", shuffled)

        tmp = ''.join(tmp)

        flag = ''

        for j in range(len(tmp)):
            for k in range(len(tmp)):
                if tmp[k] == dummy[j]:
                    flag += shuffled[k]
                    break

        print("\nFlag: ", flag)
```

Running the script yields:

```console
root@osboxes:~/Downloads/shuffle1# python3 solve.py 
922  [+] found the seed!
Dummy:  flag{ABCDEFGHIJKLMNOPQRSTU}
tmp:  UfPT}DHINQRJGMAO{gFECaSBKLl
Shuffled:  zftr}__g5y_ee0y1{graua00n1l

Flag:  flag{y0u_are_gen1051ty_0rz}
```

The flag is **flag{y0u_are_gen1051ty_0rz}**


