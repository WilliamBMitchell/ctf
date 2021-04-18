# FE04
## BRIEFING
Download the file and filter down to the username according to criteria below.

The username you are looking for has `x` as the 3rd character, followed immediately by a number from `2` to `6`, it has a `Z` character in it and the last character is `S`.

When you have the username, submit it as the flag.

Contents: 50k-users.txt

## Solution

Although the intended solution most likely involved using a regular expression, I was able to quickly identify the correct username with a short python script. I specifically used a for loop to find all usernames with 3rd character `x`, last character `S`, and which included a `Z`.

```py
with open('50k-users.txt') as f:
    lines = f.readlines()
    lines = lines
    count = 0
    for line in lines:
        line = line.strip()
        if line[-1] == 'S' and line[2] == 'x' and 'Z' in line:
            count+=1
            print line
    print count
```
Running this script produced the following output:

<pre>
root@osboxes:~/Downloads/fe04# python check.py 
xDxD0e8ZG9acIgxS
qPx71ZPhS2X0WrsS
G0xXsSJxfTZZdQhS
<b>YXx52hsi3ZQ5b9rS</b>
SCxzgWa1zx1ZRm3S
AAxoMaZnIiBWyAWS
6
</pre>

Only 6 usernames matched the 3 critiera used. Applying the 4th criteria of having the character after the `x` being a digit between 2 and 6 immediately whittled this list down to a single username.

The flag is **YXx52hsi3ZQ5b9rS**.

