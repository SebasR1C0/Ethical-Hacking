# Active hosts
```
fping -asgq 172.16.5.0/23
```
# Target Components
- CN = Common Name
- OU = Organizational Unit
- DC = Domain Component
```
CN=ACADEMY-EA-DC01,OU=Domain Controllers,DC=INLANEFREIGHT,DC=LOCAL
```
# Groups
```
crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups

python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

# Shares
```
crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'

smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```
