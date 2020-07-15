---
title: "MySQL Universalclass Lesson5"
linktitle: "MySQL Universalclass Lesson5"
summary:
date: 2020-07-15T16:10:34-04:00
lastmod: 2020-07-15T16:10:34-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 50
menu:
  mySQL_tutorials:
    name: "Lesson5 db struc"
    parent: "MySQL Universalclass"
    weight: 2020071516
---

[https://www.youtube.com/watch?v=b85eTgUrnFw&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO&index=5](https://www.youtube.com/watch?v=b85eTgUrnFw&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO&index=5)

<br>

## database storage engine
- underlying component of db manager
- creates, reads, updates, deletes (CRUD)
- importing external files
- unique file formats for different engines
- some allow file fragmentation
    - if not allow fragmentation,
    - and contiguous disk space cannot be found,
    - cannot add to database
- responsible for indexing db

<br>

### MyISAM
- prior to MySQL v5.5
- designed for speed
- does not support referential integrity, or transactions
    - i.e. if user wants to reference table1 to table2,
    - MyISAM will not check if record exists in table2
- locks entire table when a record is being modified
    - another user cannot even do look ups on that table during update process
    - e.g. MS Access
- no constraints that allow rolling back of changes
    - i.e. no UNDO
- good for tables used for SELECT statements
- NOT for UPDATE, INSERT, DELETE statements

<br>

### ARCHIVE storage engine
- stores large amount of data
- but no index
- uses little system resources
- support INSERT and SELECT
- no DELETE, REPLACE, UPDATE
- perfect for archiving data, cannot change once created

<br>

### InnoDB
- acquired by Oracle
- default storage engine for MySQL 5.5+
- transaction safe storage engine
- focus on relational integrity
- supports foreign keys, commits, rollback, crash recovery
- row-level locking
    - other users can query other rows
- handles high data volume
- use indexes
- but requires more time and processing power

<br>

### change storage engine with .cnf
- windows
    - `C:\ProgramData\MySQL\MySQL Server 5.6\my.ini`
- linux/macOSX
    - /etc/my.cnf
    - /etc/mysql/my.cnf
- change
    - default-storage-engine=INNODB
    - OR -- MYISAM
    - OR -- ARCHIVE

<br>

***
## CHECK TABLE
- verify table content, constraints met
- works with MyISAM, ARCHIVE, InnoDB
```sql
USE classicmodels;
CHECK TABLE Customers;
```

<br>

***
## REPAIR table (RECOVER)
- repairs corrupted table
- valid for MyISAM, ARCHIVE, CSV tables
- not for InnoDB
    - have to create "dump file" and reload
    - future lesson

<br>

***
## CREATE TABLE/DATABASE
```sql
CREATE DATABASE test;
USE test;

CREATE TABLE testTable
(
id INT(4) NOT NULL,                -- gonna be primary key
firstname VARCHAR(50) NOT NULL,
lastname VARCHAR(50) NOT NULL,
telephone VARCHAR(12),
email VARCHAR(50),
zipcode VARCHAR(10) NOT NULL,
PRIMARY KEY (id)
);
```

<br>

***
## DROP TABLE/DATABASE
```sql
DROP TABLE testTable;
DROP DATABASE test;
```

<br>

***
## ALTER TABLE
- modify column (add, drop, rename)
```sql
ALTER TABLE table_name
ADD column_name datatype;           -- add a column

ALTER TABLE table_name
DROP column_name;			              -- drop a column

ALTER TABLE table_name
MODIFY COLUMN column_name datatype; -- modify data type

ALTER TABLE table_name
CHANGE old_name new_name datatype;  -- rename a column
```

<br>

### examples
```sql
USE classicmodels;
```

<br>

#### change column name
```sql
ALTER TABLE Customers
CHANGE contactFirstName customerFirstName VARCHAR(50);

ALTER TABLE Customers
CHANGE contactLastName customerLastName VARCHAR(50);

SELECT * FROM Customers;
```
- CAUTION:
    - if another table **references** above columns,
    - will break database
    - try not to change tables once it is made

<br>

#### ADD COLUMN
```sql
ALTER TABLE Customers
ADD COLUMN myNewColumn VARCHAR(50);
```

<br>

#### DROP COLUMN
```sql
ALTER TABLE Customers
DROP COLUMN myNewColumn;
```

<br>

#### change data type using MODIFY
```sql
DESCRIBE Customers;

ALTER TABLE Customers
MODIFY salesRepEmployeeNumber INT(20);    -- if employee number outgrown 20 digits
```

<br>

#### check table
```sql
CHECK TABLE Customers;
-- Table: classimodels.customers
-- Msg_text: OK
```
- if want to repair, cannot repair since it's InnoDB
    - have to export into dump file,
    - import again
    - more later