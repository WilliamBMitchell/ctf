# NE01
## BRIEFING
There is a TCP network service running on `cfta-ne01.allyourbases.co`. Find it to get the flag after you connect.

Note: The target has many open ports - only one is the correct one. The correct port will identify itself with `ID: ne01` after you connect.

## Solution

**Step 1**: Scan the domain with nmap. Nmap is a free and open-source network scanner capable of discovering hosts and services on a computer network by sending packets and analyzing the responses.

```
root@osboxes:~/Downloads/fm01# nmap cfta-ne01.allyourbases.co
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-17 13:36 EDT
Nmap scan report for cfta-ne01.allyourbases.co (52.210.101.44)
Host is up (0.019s latency).
Other addresses for cfta-ne01.allyourbases.co (not scanned): 34.251.231.207
rDNS record for 52.210.101.44: ec2-52-210-101-44.eu-west-1.compute.amazonaws.com
Not shown: 999 filtered ports
PORT     STATE SERVICE
1061/tcp open  kiosk

Nmap done: 1 IP address (1 host up) scanned in 10.60 seconds
```

The nmap scan identified that port 1061 was open.

**Step 2**: Interact with the open port using netcat. Netcat is a computer networking utility for reading from and writing to network connections using TCP or UDP. It is often referred to as a " TCP/IP Swiss Army Knife"

```
root@osboxes:~/Downloads/fm01# nc cfta-ne01.allyourbases.co 1061
ID: ne01
Flag: Nmap_0f_the_W0rld!
```

The flag is **Nmap_0f_the_W0rld!**.
