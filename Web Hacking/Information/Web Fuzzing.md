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
