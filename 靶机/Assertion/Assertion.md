```bash
nmap -Pn -sV -T4 -p- 10.204.219.83 -sC
```

```bash
neon@overflow ~> nmap -Pn -sV -T4 -p- 10.204.219.83 -sC
Starting Nmap 7.99 ( https://nmap.org ) at 2026-07-02 11:58 +0800
Nmap scan report for 10.204.219.83
Host is up (0.0038s latency).
Not shown: 65530 closed tcp ports (conn-refused)
PORT      STATE    SERVICE    VERSION
53/tcp    open     domain     (generic dns response: NOTIMP)
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
1053/tcp  open     tcpwrapped
10191/tcp filtered unknown
21650/tcp filtered unknown
46302/tcp filtered unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.99%I=7%D=7/2%Time=6A45E1DD%P=x86_64-pc-linux-gnu%r(DNSVe
SF:rsionBindReqTCP,20,"\0\x1e\0\x06\x85\x84\0\x01\0\0\0\0\0\0\x07version\x
SF:04bind\0\0\x10\0\x03");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.23 seconds
```

### 很奇怪的端口




