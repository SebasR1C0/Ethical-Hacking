# Directory and File Fuzzing
```bash
sebastianrojas@sebas:~$ gobuster dir -u http://83.136.255.20:48429/webfuzzing_hidden_path/flag -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 500 -x .php,.html,.txt,.bak,.js
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://83.136.255.20:48429/webfuzzing_hidden_path/flag
[+] Method:                  GET
[+] Threads:                 500
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Extensions:              php,html,txt,bak,js
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 104]
/flag.html            (Status: 200) [Size: 100]
```
# Recursive Fuzzing
```bash
sebastianrojas@sebas:~$ ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://83.136.255.20:58452/recursive_fuzz/FUZZ -e .html -recursion

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://83.136.255.20:58452/recursive_fuzz/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
 :: Extensions       : .html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

[Status: 403, Size: 158, Words: 17, Lines: 11, Duration: 179ms]
| URL | http://83.136.255.20:58452/recursive_fuzz/
    * FUZZ: 

[Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 176ms]
| URL | http://83.136.255.20:58452/recursive_fuzz/level1
| --> | /recursive_fuzz/level1/
    * FUZZ: level1

[INFO] Adding a new job to the queue: http://83.136.255.20:58452/recursive_fuzz/level1/FUZZ
```

# Parameter and Value Fuzzing
- application/x-www-form-urlencoded: This format encodes the data as key-value pairs separated by ampersands (&), similar to GET parameters but placed within the request body instead of the URL.
- multipart/form-data: This format is used when submitting files along with other data. It divides the request body into multiple parts, each containing a specific piece of data or a file.

```bash
sebastianrojas@sebas:~$ ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -c -t 500 -u http://94.237.122.137:35299/FUZZ -mc all -e .php,.html -fs 158

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://94.237.122.137:35299/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
 :: Extensions       : .php .html 
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 500
 :: Matcher          : Response status: all
 :: Filter           : Response size: 158
________________________________________________

post.php                [Status: 404, Size: 30, Words: 4, Lines: 3, Duration: 175ms]
get.php                 [Status: 404, Size: 30, Words: 4, Lines: 3, Duration: 173ms]
```
- Post.php
```bash
sebastianrojas@sebas:~$ ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt  -ic -c -u "http://94.237.122.137:35299/post.php" -d "y=FUZZ" --mc 200 -X POST -H "Content-Type: application/x-www-form-urlencoded" -t 500

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : POST
 :: URL              : http://94.237.122.137:35299/post.php
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Data             : y=FUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 500
 :: Matcher          : Response status: 200
________________________________________________

SUNWmc                  [Status: 200, Size: 26, Words: 1, Lines: 2, Duration: 174ms]
```
```bash
curl -i -X POST -d "y=SUNWmc" "http://94.237.122.137:35299/post.php"
HTTP/1.1 200 OK
X-Powered-By: PHP/8.3.9
Content-type: text/html; charset=UTF-8
Content-Length: 26
Date: Thu, 30 Oct 2025 01:38:08 GMT
Server: lighttpd/1.4.76
```
- Get
```bash
sebastianrojas@sebas:~$ ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt  -ic -c -u "http://94.237.122.137:35299/get.php?x=FUZZ" --mc 200 -X POST -H "Content-Type: application/x-www-form-urlencoded" -t 500

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : POST
 :: URL              : http://94.237.122.137:35299/get.php?x=FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
 :: Header           : Content-Type: application/x-www-form-urlencoded
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 500
 :: Matcher          : Response status: 200
________________________________________________

OA_HTML                 [Status: 200, Size: 25, Words: 1, Lines: 2, Duration: 203ms]
```
# Virtual Host
```bash
gobuster vhost -u http://inlanefreight.htb:39526 -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt -t 200 --append-domain --exclude-status 400,401,403,404,301,302,500
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                       http://inlanefreight.htb:39526
[+] Method:                    GET
[+] Threads:                   200
[+] Wordlist:                  /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
[+] User Agent:                gobuster/3.8
[+] Timeout:                   10s
[+] Append Domain:             true
[+] Exclude Hostname Length:   false
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
ADMIN.inlanefreight.htb:39526 Status: 200 [Size: 100]
Admin.inlanefreight.htb:39526 Status: 200 [Size: 100]
admin.inlanefreight.htb:39526 Status: 200 [Size: 100]
awmdata.inlanefreight.htb:39526 Status: 200 [Size: 104]
ipdata.inlanefreight.htb:39526 Status: 200 [Size: 102]
web-beans.inlanefreight.htb:39526 Status: 200 [Size: 108]
Progress: 4746 / 4746 (100.00%)
===============================================================
Finished
===============================================================

# Subdomain Fuzzing
```
# Subdomain Fuzzing
```bash
```
