# FE02
## BRIEFING
We're sure there's a flag at `cfta-fe02.allyourbases.co` - can you find it?

## Solution

Dig (Domain Information Groper) is a command line utility that performs DNS lookup by querying name servers and displaying the results. To perform a DNS lookup for a domain name, we just pass the name to the dig command:

`dig cfta-fe02.allyourbases.co`

The output is:

```
; <<>> DiG 9.16.8-Debian <<>> cfta-fe02.allyourbases.co
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 23658
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 8d019cd297f16db3748a29bc607b166dcba889a9e3a50bcd (good)
;; QUESTION SECTION:
;cfta-fe02.allyourbases.co.	IN	A

;; ANSWER SECTION:
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.34
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.129
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.95
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.51

;; Query time: 12 msec
;; SERVER: 192.168.0.1#53(192.168.0.1)
;; WHEN: Sat Apr 17 13:10:03 EDT 2021
;; MSG SIZE  rcvd: 146
```


To query all DNS record types, we can specify the ANY option. The ANY option will include all available record types in the output:

`dig cfta-fe02.allyourbases.co ANY`

The output is:

```
; <<>> DiG 9.16.8-Debian <<>> cfta-fe02.allyourbases.co ANY
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8702
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 2d7bdb91cff6598893e6a562607b172a445836ef1061d6b0 (good)
;; QUESTION SECTION:
;cfta-fe02.allyourbases.co.	IN	ANY

;; ANSWER SECTION:
cfta-fe02.allyourbases.co. 300	IN	TXT	"flag=unlimited_free_texts"
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.51
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.95
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.129
cfta-fe02.allyourbases.co. 60	IN	A	143.204.33.34

;; Query time: 128 msec
;; SERVER: 192.168.0.1#53(192.168.0.1)
;; WHEN: Sat Apr 17 13:13:12 EDT 2021
;; MSG SIZE  rcvd: 184
```

We see that the flag is **unlimited_free_texts**.
