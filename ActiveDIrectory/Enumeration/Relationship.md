There are differentes relationship like:
- Parent-child: Two o mor domain inside the forest
- Cross-link: A trust between child domains to speed up authentication.
- External: A non-transitive trust between two separate domains in separate forests which are not already joined by a forest trust. 
- Tree-root: A two-way transitive trust between a forest root domain and a new tree root domain. 
- Forest: A transitive trust between two forest root domains.
- ESAE: A bastion forest used to manage Active Directory.

- <img width="2576" height="1666" alt="image" src="https://github.com/user-attachments/assets/179bddbd-64bf-4c7b-83a4-87c2418245b8" />

# Enumeration
```
Import-Module activedirectory
Get-ADTrust -Filter *

Get-DomainTrustMapping

netdom query /domain:inlanefreight.local trust
netdom query /domain:inlanefreight.local dc
netdom query /domain:inlanefreight.local workstation
```


## Users in the child domain
```
Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```
