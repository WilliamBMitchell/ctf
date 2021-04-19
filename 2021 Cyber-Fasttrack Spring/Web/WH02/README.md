# WH02
## BRIEFING
Access the site at https://cfta-wh02.allyourbases.co and find a way to get the flag.

## Solution

I did not get this one during the competition. Later I realized that the main page of the site included a big hint by mentioning version control.

If we use `dirb`on the site, we find the crucial directory almost immediately, that is, a `/.git` directory:

```console
root@osboxes:/usr/share/wordlists/dirbuster# dirb https://cfta-wh02.allyourbases.co/

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sun Apr 18 22:03:43 2021
URL_BASE: https://cfta-wh02.allyourbases.co/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: https://cfta-wh02.allyourbases.co/ ----
+ https://cfta-wh02.allyourbases.co/.git/HEAD (CODE:200|SIZE:23)  
```
