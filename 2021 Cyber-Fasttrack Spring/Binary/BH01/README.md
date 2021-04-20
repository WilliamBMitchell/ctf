# BH01
## BRIEFING
Download the file and find a way to get the flag.

Contents: program

## Solution

I believe I solved this one in an unintended manner. I first ran the program to see what I was wording with:

```console
root@osboxes:~/Downloads/bh01# ./bh01
What is the magic word?
asdf
�:�lO?d#����*���E	
Did you understand that?
```

I tried a few short inputs and got the same output each time. During the competition I didn't notice that ouput could vary based on input. I suspected that there was a specific magic word we might need. I went ahead and opened the program in IDA (free version). IDA stands for Interactive DisAssembler, which is a disassembler for computer software which generates assembly language source code from machine-executable code. I like IDA because it displays the different functions of a program in a visually appealing format and draws arrows between blocks to tell you where one function points to another.

I immedatiately honed in on the main function:

<img src="main.png" width="350">

Inside the main function I found what appeared to be an encrypt function and a decrypt function.

Encrypt:


Decrypt:



I noticed that, when execute, the program only ran through what I had guessed was the encrypt function. I decided to patch the final instruction of the encrypt function to re-direct the program into what I had guessed was the decrypt function.

I changed the instruction from `js` to `jns`:

I then applied the patch to the program and re-ran the program:

```console
root@osboxes:~/Downloads/bh01# ./bh01_ida 
What is the magic word?
asdf
Segmentation fault
```

I got a `Segmentation fault`. Since I was pretty confident I had applied the patch correctly I decided to run the program in gdb to see if that flag might be located in memory. Sure enough:

```console
root@osboxes:~/Downloads/bh01# gdb-gef ./bh01_ida
Reading symbols from ./bh01_ida...
(No debugging symbols found in ./bh01_ida)
GEF for linux ready, type `gef' to start, `gef config' to configure
77 commands loaded for GDB 10.1.90.20210103-git using Python engine 3.9
[*] 3 commands could not be loaded, run `gef missing` to know why.
gef➤  r
Starting program: /root/Downloads/bh01/bh01_ida 
What is the magic word?
asdf

Program received signal SIGSEGV, Segmentation fault.
0x0000555555555343 in ?? ()

[ Legend: Modified register | Code | Heap | Stack | String ]
─────────────────────────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0xfb0             
$rbx   : 0x0               
$rcx   : 0x2               
$rdx   : 0x14              
$rsp   : 0x00007fffffffe030  →  0x1400000000000000
$rbp   : 0x00007fffffffe0c0  →  0xff55bd8c747f0951
$rsi   : 0x00007fffffffe004  →  0xa5e18600716bb06f
$rdi   : 0x00007ffff7f9a740  →  0x00007ffff7f9a1d4  →  0x20ca240561fc6841
$rip   : 0x0000555555555343  →   movzx eax, BYTE PTR [rbp+rax*1-0x70]
$r8    : 0x00007ffff7f9a1d4  →  0x20ca240561fc6841
$r9    : 0x00007ffff7f9a240  →  0x0000000000000008
$r10   : 0x6e              
$r11   : 0x246             
$r12   : 0x0000555555555120  →   endbr64 
$r13   : 0x0               
$r14   : 0x0               
$r15   : 0x0               
$eflags: [zero CARRY PARITY ADJUST sign trap INTERRUPT direction overflow RESUME virtualx86 identification]
$cs: 0x0033 $ss: 0x002b $ds: 0x0000 $es: 0x0000 $fs: 0x0000 $gs: 0x0000 
─────────────────────────────────────────────────────────────────────────────────────────────────────────────── stack ────
0x00007fffffffe030│+0x0000: 0x1400000000000000	 ← $rsp
0x00007fffffffe038│+0x0008: 0xffffffa600000fb0
0x00007fffffffe040│+0x0010: 0x00000000607cf17a
0x00007fffffffe048│+0x0018: 0x1816120a05000000
0x00007fffffffe050│+0x0020: "Flag: aLittLeObfuScatIonalCharActEr"
0x00007fffffffe058│+0x0028: "ittLeObfuScatIonalCharActEr"
0x00007fffffffe060│+0x0030: "uScatIonalCharActEr"
0x00007fffffffe068│+0x0038: "alCharActEr"
───────────────────────────────────────────────────────────────────────────────────────────────────────── code:x86:64 ────
   0x55555555532e                  mov    DWORD PTR [rbp-0x88], 0x0
   0x555555555338                  jmp    0x55555555560c
   0x55555555533d                  mov    eax, DWORD PTR [rbp-0x88]
 → 0x555555555343                  movzx  eax, BYTE PTR [rbp+rax*1-0x70]
   0x555555555348                  mov    BYTE PTR [rbp-0x89], al
   0x55555555534e                  not    BYTE PTR [rbp-0x89]
   0x555555555354                  add    BYTE PTR [rbp-0x89], 0x2f
   0x55555555535b                  movzx  eax, BYTE PTR [rbp-0x89]
   0x555555555362                  shr    al, 1
───────────────────────────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "bh01_ida", stopped 0x555555555343 in ?? (), reason: SIGSEGV
─────────────────────────────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x555555555343 → movzx eax, BYTE PTR [rbp+rax*1-0x70]
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
gef➤  quit
```

The flag is **aLittLeObfuScatIonalCharActEr**.
