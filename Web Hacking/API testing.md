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
