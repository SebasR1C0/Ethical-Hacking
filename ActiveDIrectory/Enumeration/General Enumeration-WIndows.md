# Basic
- hostname or whoammi
- systeminfo
- Get-MpComputerStatus
- qwinsta -> Curent users in the domain
- [System.Environment]::OSVersion.Version -> Prints out the OS version and revision level
- wmic qfe get Caption,Description,HotFixID,InstalledOn	-> Prints the patches and hotfixes applied to the host
- ipconfig /all	-> Prints out network adapter state and configurations
- set	-> Displays a list of environment variables for the current session (ran from CMD-prompt)
- echo %USERDOMAIN%	-> Displays the domain name to which the host belongs (ran from CMD-prompt)
- echo %logonserver%	-> Prints out the name of the Domain controller the host checks in with (ran from CMD-prompt)

# Modules
```
Get-Module

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0.1.0    ActiveDirectory                     {Add-ADCentralAccessPolicyMember, Add-ADComputerServiceAcc...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

# Checking Defenses
```
netsh advfirewall show allprofiles
# Windows defender
sc query windefend
```

# Network
```
arp -a	
ipconfig /all	
route print
netsh advfirewall show allprofiles
```

# WMI
```
mic qfe get Caption,Description,HotFixID,InstalledOn	Prints the patch level and description of the Hotfixes applied
wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List
```

# Net Commands
```
net accounts
net accounts /domain
net group /domain
```

# Dsquery
```
dsquery user
dsquery computer
dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"

# LDAP (No password)
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl

# Domain Controller
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
