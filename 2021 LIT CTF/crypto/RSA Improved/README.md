## Problem ##

The standard RSA only uses 2 prime factors, way too insecure. Let’s improve it by using more factors! More is better, ...right?

Ouput.txt:
```
n = 10588750243470683238253385410274703579658358849388292003988652883382013203466393057371661939626562904071765474423122767301289214711332944602077015274586262780328721640431549232327069314664449442016399
e = 65537
ct = 5995952936037255929781924635247478684210608634033130708312547257115162490907542249878843535087479397093661825467058312432783733583919194527896596274111265902276347768535338414466405501311805051241244
```

## Solution ##

The key of this problem is that `n` has more than 2 prime factors. If we take `n` to [factordb](http://factordb.com/) we find that `n` is the product of 21 primes.

Here is the source code:

```py
from Crypto.Util.number import getPrime
import math

# string to decimal
pt = int(open('flag.txt','rb').read().hex(),16);

primes = [];
n = 1
# euler totient
phi = 1
# public key
e = 65537

while math.log2(n) < 640:
	primes.append(getPrime(32));
	n *= (primes[-1]);
	phi *= (primes[-1] - 1);

# No duplicates
assert(len(primes) == len(list(set(primes))));
# private key
d = pow(e,-1,phi);
# cipher text
ct = pow(pt,e,n);

def decrypt(ct):
	# decode ciphertext for plaintext
	pt = pow(ct,d,n);
	# convert decimal back to string
	return bytes.fromhex(hex()[2:]).decode("utf-8");

print("n = " + str(n));
print("e = 65537");
print("ct = " + str(ct));
```

I found some good discussion on why an `n` with more than 2 factors leads to weak RSA [here](https://crypto.stackexchange.com/questions/35440/why-does-rsa-need-p-and-q-to-be-prime-numbers). I also found some good discussion on how to solve RSA when `n` is the product of 3 primes [here](https://crypto.stackexchange.com/questions/44110/rsa-with-3-primes); the argument naturally extended itself to more primes.

And here is my solve script:

```py
import math

# multiplicate inverse of a mod m using extended euclidean algorithm
def modInverse(a, m):
    m0 = m
    y = 0
    x = 1
 
    if (m == 1):
        return 0
 
    while (a > 1):
 
        # q is quotient
        q = a // m
 
        t = m
 
        # m is remainder now, process
        # same as Euclid's algo
        m = a % m
        a = t
        t = y
 
        # Update x and y
        y = x - q * y
        x = t
 
    # Make x positive
    if (x < 0):
        x = x + m0
 
    return x


ct = 5995952936037255929781924635247478684210608634033130708312547257115162490907542249878843535087479397093661825467058312432783733583919194527896596274111265902276347768535338414466405501311805051241244

e = 65537

n = 10588750243470683238253385410274703579658358849388292003988652883382013203466393057371661939626562904071765474423122767301289214711332944602077015274586262780328721640431549232327069314664449442016399

# arr pulled from factordb based on n
arr = [2151055861, 2319991937, 2341310833, 2391906757, 2448497717, 2493514393, 2586983803, 2758321513, 2784469417, 2816940109, 2865965429, 3092165243, 3218701459, 3438516511, 3526137361, 3663803701, 3673658161, 3789550043, 3866428117, 3919632263, 4147385899]


#totient(n) = lcm( totient(p), totient(q), totient(r), ... , totient(z) )
totients = []
for val in arr:
    totients.append(val-1)

gcd = math.gcd(totients[0],totients[1])

for i in range(2,len(totients)):
    gcd = math.gcd(gcd,totients[i])

lcm = 1
for val in totients:
    lcm *= val
lcm = lcm // gcd

totient_n = lcm


d = modInverse(e,totient_n)

pt = hex(pow(ct,d,n))

print( bytearray.fromhex(pt[2:]).decode())
```

Running yields the flag:

```console
root@osboxes:~/Downloads/RSA Improved# python3 scratchwork.py 
flag{rsa_1s_4_pr3tty_imp0rt4nt_crypt0_4lg0r1thm}
```

The flag is **flag{rsa_1s_4_pr3tty_imp0rt4nt_crypt0_4lg0r1thm}**

