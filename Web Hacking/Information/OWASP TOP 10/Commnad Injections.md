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
| Background         | `&`                 | `%26`                 |
| Pipe               | `|`                 | `%7c`                 |
| AND                | `&&`                | `%26%26`              |
| OR                 | `||`                | `%7c%7c`              |
| Sub-Shell          | `` `..` ``          | `%60%60`              |
| Sub-Shell          | `$()`                | `%24%28%29`           |
