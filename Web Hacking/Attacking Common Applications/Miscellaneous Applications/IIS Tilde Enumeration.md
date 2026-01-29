# Custom directory 
```
egrep -r ^transf /usr/share/wordlists/* | sed 's/^[^:]*://' > /tmp/list.txt
gobuster dir -u http://10.129.204.231/ -w /tmp/list.txt -x .aspx,.asp
```


# Tool
```
java -jar iis_shortname_scanner.jar 0 5 http://10.129.204.231/
```
