# Kerbrute
Idetifying users pre-authentication in Kerberos
```
kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```
# Domain user
- Null session
```
rpcclient -U "" -N 10.129.95.210 -c "enumdomusers" | awk -F'[][]' '{print $2}' > users.txt
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]" > users.txt
crackmapexec smb 172.16.5.5 --users
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" " > users.txt
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```
- With credentials

```
crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```
