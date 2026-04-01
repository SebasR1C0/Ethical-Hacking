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
