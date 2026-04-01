# Tools


# User Information
```
net user
net localgroup or net localgroup administrators
```
# Privileges
```
whomai /all

# Hashes
icacls c:\Windows\System32\config\SAM

# APPS
icacls "c:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
```

# System Information
```
systeminfo

# Windows version
[environment]::OSVersion.Version

# Enumeration 
set
```

# Services
```
# Services
netstat -ano

# Processes
tasklist /svc
pipelist.exe /accepteula or gci \\.\pipe\ or accesschk.exe /accepteula \\.\Pipe\lsass -v

# Installed Programs
wmic product get name
Get-WmiObject -Class Win32_Product |  select Name, Version

# Spoolss (CVE-2021-1675.ps1)
ls \\localhost\pipe\spoolss
Set-ExecutionPolicy Bypass -Scope Process


# Services uploads
wmic qfe list brief
Get-HotFix | ft -AutoSize
```

# Common Files
- .*
- *_history or _hist
- *.bak
- *.conf -o -name .config
- *.py -o -name .sh
Code
```
find / -type f \( -iname *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```

# Network
```
ipconfig /all
arp -a
route print
```

# EDR
```
# Identifying Windows Defender
Get-MpComputerStatus

# AppLocker
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path 
```

# Uncommon attacks
- [Privileges](https://github.com/SebasR1C0/Ethical-Hacking/blob/main/ActiveDIrectory/Explotation/BloodHound.md)
