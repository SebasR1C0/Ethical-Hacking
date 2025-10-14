# Active Directory
Group of users and computers under the administration of a given business (Winodws Domain)
- Centralised identity management
- Managing security policies

Domain Contoller (DC) is in charge of running AD services

The core of a any Windows Domain is the Active Directory Domain Service (AD DS).

The machine account name is the computer's name followed by a dollar sign

Authentication Methods:
- Kerberos
- NetNTLM

## KERBEROS AUTHENTICATION
Kerberos authentication is the default authentication protocol for any recent version of Windows. Users who log into a service using Kerberos will be assigned tickets. Steps:
1. The user sends their username and a timestamp encrypted using a key derived from their password to the Key DIstribution Center (KDC)
2. KDS will create a Tickect Granting TIcket (TGT) or a Session Key, thisl will allows to request additional tickets to access specific services.
3. TGT is encrypted by krbtgt
4. With the TGT, we will be able to ask the KDC for a Ticket Granting Service (TGS)
