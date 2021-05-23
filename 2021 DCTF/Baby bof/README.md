Baby bof was a 250 point challenge. When we open the provided binary in ghidra we find the below vuln() function:

```c
void vuln(void)

{
  char local_12 [10];
  
  puts("plz don\'t rop me");
  fgets(local_12,0x100,stdin);
  puts("i don\'t think this will work");
  return;
}
```
