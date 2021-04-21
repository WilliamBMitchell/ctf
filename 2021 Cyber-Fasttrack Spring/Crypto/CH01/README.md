# CH01
## BRIEFING
Download the files and find a way to retrieve the encrypted data.

Contents: 1.pub, 2.pub, m1.enc, m2.enc

## Solution

I did not get this one during the competition but found a swift way later on. Given that we were provided 2 public keys and 2 encoded messages, one could hypothesize an RSA Common Modulus Attack might be in play if they knew such an attack required these exact items.

There are some tools out there, such as [here](https://github.com/HexPandaa/RSA-Common-Modulus-Attack/tree/2c659a344746862ecb8a12c20ecae3897cc89f1c) that will take the 4 provided files and spit out the flag:

```console
root@osboxes:~/Downloads/ch01# python3 rsa-cm.py -c1 m1.enc -c2 m2.enc -k1 1.pub -k2 2.pub
[+] Recovered message:
8890125754887937411828077954731805906286490380137883135984143260551703819734842400222717478646646077726129754152536477411210370477872730243768640237139024274761857845969320109130732059872358070976283518327690405764700575068395929876003269646407806676159415224498086016969007643394525867668303737918845568202735259258227280892282760950054976219763159884043733610551287602341292147384860085214837257222191875036612558464281771937205223471518801238849738303991791717613596567715589016505529736013585710651567970584890437169998475070192492375054693957775149134444634185125814262316852761032851538733940990589701231950368
[+] Recovered bytes:
b'Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: shaRinGisCaRinG-010Flag: '
```

The flag is **shaRinGisCaRinG-010**.
