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
Information in PS history: 
```
(Get-PSReadLineOption).HistorySavePath  
```
All users
```
foreach($user in ((ls C:\users).fullname)){cat "$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt" -ErrorAction SilentlyContinue}
```
- .*
- *_history or _hist
- *.bak
- *.py
- .kdbx
- .vmdk
- .vdhx
- .ppk
- .sqlite*
- .txt
- .ini
- .cfg
- .config
- .xml
Code
```
findstr /SI /M "password" *.xml *.ini *.txt
findstr /si password *.xml *.ini *.txt *.config

dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == 
where /R C:\ *.config
#PS
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore
```
EXAMPLES
```
%SYSTEMDRIVE%\pagefile.sys
%WINDIR%\debug\NetSetup.log
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\iis6.log
%WINDIR%\system32\config\AppEvent.Evt
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
%WINDIR%\system32\CCM\logs\*.log
%USERPROFILE%\ntuser.dat
%USERPROFILE%\LocalS~1\Tempor~1\Content.IE5\index.dat
%WINDIR%\System32\drivers\etc\hosts
C:\ProgramData\Configs\*
C:\Program Files\Windows PowerShell\*
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
