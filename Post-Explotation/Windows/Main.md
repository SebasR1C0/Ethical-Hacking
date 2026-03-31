# Tools


# User Information
```
```
# Privileges
```

```

# System Information
```

```

# Common Files
- .*
- *_history or _hist
- *.bak
- *.conf -o -name .config
- *.py -o -name .sh
Code
```
find / -type f \( -iname *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```

# Network
```
ipconfig /all
arp -a
route print
```

# EDR
```
# Identifying Windows Defender
Get-MpComputerStatus

# AppLocker
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path 
```

# Uncommon attacks

