# Discover Modules
```
Get-Module
```
Importing AD modules
```
Import-Module ActiveDirectory
```

# Domain info
```
Get-ADDomain
```

# User
```
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol

# Posibkle kerbrosting attack
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

# Groups
```
Get-ADGroup -Filter * | select name
Get-ADGroup -Identity "Backup Operators"

# Identify all members
Get-ADGroupMember -Identity "Backup Operators"

# Identify all members and indirect members
Get-ADGroupMember -Identity "Backup Operators" -Recurse
```

# Domains
Numeration relationships between domains
```
Get-DomainTrustMapping
```

# Admin
```
Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```
# Tools
## SharpView
```
 .\SharpView.exe Get-DomainUser -Identity forend
```
## Snaffler
Searching private information on shares smb
```
 .\Snaffler.exe  -d INLANEFREIGHT.LOCAL -s -v data
```
# BloodHound
```
 .\SharpHound.exe -c All --zipfilename ILFREIGHT
```
