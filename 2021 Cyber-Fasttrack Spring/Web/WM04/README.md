# WM04
## BRIEFING
Visit the Italian dish suggestion site at https://cfta-wm04.allyourbases.co and find a way to get the flag.

## Solution

The initial page presents us with a search box to ostensibly for us to search for ingredients for Italian dishes:

<img src="italian.png" width="500">

After playing around a little bit I checked whether the search field might be vulnerable to Server Side Template Injection (SSTI). The payload for the test was `{{7*7}}`:

<img src="ssti.PNG" width="500">

Sure enough, the computation executed and displayed the result (`49`). After attempting a few payloads I found one that displayed the flag:

<img src="wm04.png" width="500">

The flag is **t3mpl4te_vu1n**.
