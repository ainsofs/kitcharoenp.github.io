---
layout: post
title: "MySQL : Install MySQL 8.0 on Ubuntu"
published: true
categories: [mysql]
---
Notes while reading : [Install MySQL 8.0 on Ubuntu](https://linoxide.com/linux-how-to/install-mysql-ubuntu/)

### Download repository from [MySQL Community Downloads](https://dev.mysql.com/downloads/repo/apt/)
```
curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.14-1_all.deb
```

### Install MySQL repository package.
Scroll down and select the last option - **"Ok"**
```
sudo dpkg -i mysql-apt-config_0.8.14-1_all.deb

sudo apt update
```

### Install MySQL server
```
sudo apt install mysql-server
```

### Display the information
```
root@mysql-repl:~# mysqladmin -u root -p version
Enter password:
mysqladmin  Ver 8.0.18 for Linux on x86_64 (MySQL Community Server - GPL)
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version		8.0.18
Protocol version	10
Connection		Localhost via UNIX socket
UNIX socket		/var/run/mysqld/mysqld.sock
Uptime:			1 min 22 sec

Threads: 2  Questions: 3  Slow queries: 0  Opens: 115  Flush tables: 3  Open tables: 35  Queries per second avg: 0.036
```

### Install MySQL Shell
> [MySQL Shell](https://dev.mysql.com/doc/mysql-shell/8.0/en/) is an advanced client and code editor for MySQL Server.
> * MySQL Shell processes code in the following languages: JavaScript, Python and SQL.
> * Interactive Code Execution
> * Batch Code Execution
> * Supported APIs
> * ...

```
sudo apt install mysql-shell
```
Test local login :   `mysqlsh root@localhost`
```
$ mysqlsh root@localhost
mysqlsh root@localhost
Please provide the password for 'root@localhost': ********
Save password for 'root@localhost'? [Y]es/[N]o/Ne[v]er (default No):
MySQL Shell 8.0.18

Copyright (c) 2016, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its affiliates.
Other names may be trademarks of their respective owners.

Type '\help' or '\?' for help; '\quit' to exit.
Creating a session to 'root@localhost'
Fetching schema names for autocompletion... Press ^C to stop.
Your MySQL connection id is 9 (X protocol)
Server version: 8.0.18 MySQL Community Server - GPL
No default schema selected; type \use <schema> to set one.
MySQL  localhost:33060+ ssl  JS >
```
Switching to SQL mode:  `\sql`
```
 MySQL  localhost:33060+ ssl  JS > \sql
Switching to SQL mode... Commands end with ;
 MySQL  localhost:33060+ ssl  SQL > show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.0433 sec)
 MySQL  localhost:33060+ ssl  SQL >
```

### Securing installation
* Remove anonymous users?
* Disallow root login remotely?
* Remove test database and access to it?

```
$ sudo mysql_secure_installation
```

### Fixed
**`The server requested authentication method unknown to the client`**

```
# Command to create the new user with the old authentication:
CREATE USER newuser@"%" IDENTIFIED WITH mysql_native_password BY'password';

#Command to grant permissions to the new user:
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%';
FLUSH PRIVILEGES;
```

**`SQLSTATE[22007]: Invalid datetime format: 1292 Incorrect date value: '0000-00-00'`**

To Check MYSQL mode
```sql
mysql> SELECT @@GLOBAL.sql_mode global, @@SESSION.sql_mode session \G
*************************** 1. row ***************************
 global: ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
session: ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
1 row in set (0.00 sec)
```

Disabling NO_ZERO_DATE, NO_ZERO_IN_DATE mode

SET GLOBAL sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';




[1]: https://stackoverflow.com/questions/36374335/error-in-mysql-when-setting-default-value-for-date-or-datetime/36374690#36374690 "SQL Incorrect date value"
