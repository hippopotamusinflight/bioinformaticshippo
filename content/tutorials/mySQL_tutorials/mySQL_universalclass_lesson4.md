---
title: "MySQL Universalclass Lesson4"
linktitle: "MySQL Universalclass Lesson4"
summary:
date: 2020-07-15T14:40:33-04:00
lastmod: 2020-07-15T14:40:33-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 40
menu:
  mySQL_tutorials:
    name: "Lesson4 basic SQL"
    parent: "MySQL Universalclass"
    weight: 2020071515
---

[https://www.youtube.com/watch?v=zAUzK3yBCZc](https://www.youtube.com/watch?v=zAUzK3yBCZc)

## start server, workbench
- start MySQL server from system preferences
- start MySQL workbench
- open database (Local instance 3306)

<br>

***
## database level commands

- `SHOW databases;` (shows all databases, including some MySQL backend dbs)
- `CREATE database test;`
- `DROP database test;`
- `USE classicmodels;` (now all commands will go to classicmodels, schema bolded)
- `SHOW TABLES;`
- (cmd+enter to run a line, cmd+shift+enter to run all)

<br>

## SQL commands
- SELECT col FROM table WHERE col=val;
- INSERT INTO table (col1,col2 ...) VALUES(col1 val, col2 val ...);
- UPDATE table SET col1=val WHERE col2=val;
- DESCRIBE table;
- DELETE col FROM table WHERE col=val;

<br>

### SELECT commands

```sql
SELECT customerName, customerNumber, contactLastName, contactFirstName
FROM Customers;

SELECT *
FROM Customers
WHERE customerNumber = 121;

SELECT customerName, phone
FROM Customers
WHERE customerNumber = 121;
```

<br>

### DESCRIBE a table
```sql
DESCRIBE Customers;
```  
<img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson4_files/describe1.png" alt="" width="400px"/>  

- CHAR(50) means value has to be 50 char long
- VARCHAR(50) means value can be 1 to 50 char long
- Null column can be set to YES or NO
     - NO means Null value is not allowed
     - (`firstname VARCHAR(50) NOT NULL`)
- can use this to see what columns can be Null
     - when inserting new entries

<br>

### INSERT commands
- order matters
- NULL if record does not have an attribute
```sql
-- insert a new customer
INSERT INTO Customers
VALUES (2001, 'Hometown Baker', 'Smith', 'Bob', 
'555-222-1212', '123 Main St.', NULL, 'Orlando', 'FL',
32001, 'USA', 1370, 22000);

SELECT *
FROM Customers
WHERE customerNumber = 2001;
```

<br>

### UPDATE commands
```sql
-- update customer phone number
UPDATE Customers
SET phone = '555-555-1212'
WHERE customerNumber = 2001;
```

<br>

### INSERT, specifying columns
```sql
INSERT INTO Customers
-- order does not matter,
-- but VALUES order have to match
(
customerNumber, customerName, contactlastName, contactFirstname, addressLine1, city, phone, country
)
VALUES
(
2002, 'Betty\'s Pancakes', 'Doe', 'Betty', '222 2nd St.', 'Orlando', '555-234-1212', 'USA'
);
```

<br>

### default is NULL
```sql
-- values not entered default to NULL
SELECT *
FROM Customers
WHERE customerNumber = 2002;

-- update customer state from Null to 'FL'
UPDATE Customers
SET state = 'FL'
WHERE customerNumber = 2002;

-- update customer postalCode from Null to 32801
UPDATE Customers
SET postalCode = 32801
WHERE customerNumber = 2002;
```

<br>

### DELETE commands
```sql
-- delete record (do SELECT * FROM.. WHERE.. first to double check)
DELETE
FROM Customers
WHERE customerNumber = 2002;
```