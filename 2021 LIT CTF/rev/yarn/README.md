## Problem ##

Cats like yarn >.<

## Solution ##

We run `strings` against the binary for the flag:

```console
root@osboxes:~/Downloads/yarn# strings yarn | grep flag
However, cats also love flags, and you won't find it by running this script.
Where else could the flag be hiding? Please hurry and find it for the poor kitties!
flag{y4rn_4nd_s1lk_4r3_n1c3_but_str1ngs_1s_wh4t_g0t_th3_fl4g}
```

The flag is **flag{y4rn_4nd_s1lk_4r3_n1c3_but_str1ngs_1s_wh4t_g0t_th3_fl4g}**
