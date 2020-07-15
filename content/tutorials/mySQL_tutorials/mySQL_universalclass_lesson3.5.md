---
title: "MySQL Universalclass Lesson3.5"
linktitle: "MySQL Universalclass Lesson3.5"
summary:
date: 2020-07-15T15:22:06-04:00
lastmod: 2020-07-15T15:22:06-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 35
menu:
  mySQL_tutorials:
    name: "Lesson3.5 import db"
    parent: "MySQL Universalclass"
    weight: 2020071514
---

## import classicmodels database
- [http://www.eclipse.org/birt/documentation/sample-database.php](http://www.eclipse.org/birt/documentation/sample-database.php)
- put ClassicModels-MySQL folder into `/usr/local/mysql-8.0.16-macos10.14-x86_64/`

<br>

## source .sql scripts
```
SOURCE /usr/local/mysql-8.0.16-macos10.14-x86_64/ClassicModels-MySQL/scripts/create_classicmodels.sql;
SOURCE /usr/local/mysql-8.0.16-macos10.14-x86_64/ClassicModels-MySQL/scripts/load_classicmodels.sql;
```  
- `SOURCE` command is not a MySQL statement, but something only handled by the MySQL client
- MySQL Workbench does not handle this (as it is focused on pure MySQL code)

<br>

## MySQL in terminal
```
mysql -u root -p             # login
quit                         # quit

mysql> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| classicmodels      |
| company            |
| information_schema |
| mysql              |
| performance_schema |
| students           |
| sys                |
+--------------------+
7 rows in set (0.00 sec)
```


-- show all databases
SHOW databases;

-- use a database (schema will be bolded)

CREATE DATABASE classicmodels;
USE classicmodels;


<br>

***
## issues

create_classicmodels.sql; worked fine
load_classicmodels.sql; throws error
Error Code: 1148. The used command is not allowed with this MySQL version

#### NOT a solution
https://stackoverflow.com/questions/18437689/error-1148-the-used-command-is-not-allowed-with-this-mysql-version
SHOW VARIABLES LIKE 'local_infile';
- outputs OFF

SET GLOBAL local_infile = 1;
SHOW VARIABLES LIKE 'local_infile';
- now outputs ON

-- still doesn't work...
LOAD DATA LOCAL INFILE '../datafiles/customers.txt' INTO TABLE Customers
	FIELDS TERMINATED BY ',' ENCLOSED BY '"' LINES TERMINATED BY '\r\n';

-- tried LOAD DATA INFILE, doesn't work

#### solution?
https://stackoverflow.com/questions/32737478/how-should-i-tackle-secure-file-priv-in-mysql
- MySQL server has been started with --secure-file-priv option which basically limits 
	from which directories you can load files using LOAD DATA INFILE

SHOW VARIABLES LIKE "secure_file_priv";
- outputs NULL, no folder??? great...

#### solution?
https://stackoverflow.com/questions/10757169/location-of-my-cnf-file-on-macos/10757312
minghan:~$ mysql --help or mysql --help | grep my.cnf
                      order of preference, my.cnf, $MYSQL_TCP_PORT,
/etc/my.cnf /etc/mysql/my.cnf /usr/local/mysql/etc/my.cnf ~/.my.cnf 


<br>

***
## import database with GUI
- could not load using SQL statements...
- had to use mysql workbench GUI
    - right click on "Customers" table
    - had to add a fake header, mysql wb cannot read no header...
    - change datafiles/Customers.txt to .csv file
    - must use latin-1 encoding
- repeat for all tables...
- maybe later can figure out how to config my.cnf...
- Product.csv had to be edited, changed e.g. 8"" to 8inches
