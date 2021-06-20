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
