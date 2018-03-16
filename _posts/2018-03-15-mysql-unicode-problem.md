---
layout: post
title: Unicode Problem on MySQL
tags: [MySQL, Unicode]
---

When using MySQL as you database engine, either locally or on the cloud (AWS RDS), there might be `UnicodeEncodeError` raising from MySQL, rather than from python/Django:

```python
Internal Server Error: /url/path/operation/id

UnicodeEncodeError at /url/path/operation/id
'ascii' codec can't encode characters in position 109-112: ordinal not in range(128)
```

This is due to the **charset** and **collation** problem on MySQL database on the server side. For example, if you deployed your website to AWS EC2, the default character set and collation are `latin1` and `latin1_swedish_ci`. Any character sending from the client side with Unicode encoding, MySQL will raise the above error. To resolve this problem, we need to change the encoding to `utf8/utf8mb4`. `utf8mb4` (including astral symbols) is highly recommended to avoid any Unicode errors, though it is a little slower than `utf8`.

Before doing anything operation to your MySQL database, **BACKUP! BACKUP! BACKUP!** .

Assume that, you are using MySQL on you EC2 instance, rather than the RDS service, to save money. You should use SSH to connect to your AWS EC2 instance first for could hosting first:

```bash
ssh -i your-pem-file.pem uname@your.domain.com
```

Then, backup your database.

```sql
mysqldump -u root -p dbName > dbName_bak_date_keywords.sql
```

To check the character set of your MySQL database, use the following command:

```sql
mysql> SHOW VARIABLES WHERE Variable_name LIKE 'character\_set\_%' OR Variable_name LIKE 'collation%';
```

You will be prompt with the following table

```
+--------------------------+-------------------+
| Variable_name            | Value             |
| ------------------------ | ----------------- |
| character_set_client     | latin1            |
| character_set_connection | latin1            |
| character_set_database   | latin1            |
| character_set_filesystem | binary            |
| character_set_results    | latin1            |
| character_set_server     | latin1            |
| character_set_system     | utf8              |
| collation_connection     | latin1_swedish_ci |
| collation_database       | latin1_swedish_ci |
| collation_server         | latin1_swedish_ci |
+--------------------------+-------------------+
10 rows in set (0.00 sec)
```

If this is your case, you need to change them to support Unicode. To make the change, you will be using the following `ALTER TABLE` command.

```sql
# For each database:
ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;

# For each table:
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

# For each column:
ALTER TABLE table_name CHANGE column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Don’t blindly copy-paste this! The exact statement depends on the column type, maximum length, and other properties. The above line is just an example for a `VARCHAR` column.

After applying to each table, you should be getting the following table:

```
+--------------------------+-------------------+
| Variable_name            | Value             |
| ------------------------ | ----------------- |
| character_set_client     | utf8mb4           |
| character_set_connection | utf8mb4           |
| character_set_database   | utf8mb4           |
| character_set_filesystem | binary            |
| character_set_results    | utf8mb4           |
| character_set_server     | utf8mb4           |
| character_set_system     | utf8              |
| collation_connection     | utf8mb4_unicode_ci|
| collation_database       | utf8mb4_unicode_ci|
| collation_server         | utf8mb4_unicode_ci|
+--------------------------+-------------------+
10 rows in set (0.00 sec)
```

Please be aware, the `character_set_filesystem` and `character_set_system` cannot be changed. Of course, you can use `utf8` for some settings, as long as you are sure there is no astral symbols involved.

Similarly, a very long `VARCHAR` column may need to be changed to one of the longer `TEXT` types if you want to convert it from `utf8` to `utf8mb4`.

InnoDB has a maximum index length of `767` bytes, so for `utf8` or `utf8mb4` columns, you can index a maximum of `255` or `191` characters, respectively. If you currently have `utf8` columns with indexes longer than `191` characters, you will need to index a smaller number of characters. In an InnoDB table, these column and index definitions are legal:

```sql
col1 VARCHAR(500) CHARACTER SET utf8, INDEX (col1(255))
```

To use `utf8mb4` instead, the index must be smaller:

```sql
col1 VARCHAR(500) CHARACTER SET utf8mb4, INDEX (col1(191))
```

The preceding types of changes are most likely to be required only if you have very long columns or indexes. Otherwise, you should be able to convert your tables from `utf8` to `utf8mb4` without problems.

## References

- [MySQL Mariadb - Set character Set and Collation to utf8](• https://scottlinux.com/2017/03/04/mysql-mariadb-set-character-set-and-collation-to-utf8/)
- [MySQL utf8mb4](https://mathiasbynens.be/notes/mysql-utf8mb4)
