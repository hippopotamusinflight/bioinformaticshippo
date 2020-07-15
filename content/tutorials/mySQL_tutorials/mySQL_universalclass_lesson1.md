---
title: "MySQL Universalclass Lesson1"
linktitle: "MySQL Universalclass Lesson1"
summary:
date: 2020-07-15T10:28:51-04:00
lastmod: 2020-07-15T10:28:51-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 20
menu:
  mySQL_tutorials:
    name: "Lesson1 overview"
    parent: "MySQL Universalclass"
    weight: 2020071509
---

[https://www.youtube.com/watch?v=FhbCS6lx2wg&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO](https://www.youtube.com/watch?v=FhbCS6lx2wg&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO)

<br>

## objectives
1. what is a database
2. why use a database
3. what's in a database
4. what does a database look like
5. what is a DBMS
6. how is data retrieved/updated
7. history of SQL
8. pros/cons of different DBMS

<br>

## what is a database
- collection of files, organized in a logical way
- allow fast update/retrieval
- NOT a spreadsheet { many spreadsheets linked? with user access functions? }
    - { ohh, auto-update for multi-user read/write }

<br>

## why use a database
- allow multi-user read/write
- updated data available to all users (with access)

<br>

## what's in a database
- parts of a database
    - tables (rows - records / columns - attributes)
    - stored procedures
    - views
    - user logins/ roles

<br>

## representations of a database
- entity-relationship diagram (ERD)  
    <img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson1_files/ERD.png" alt="" width="150px"/>

- table representation  
    <img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson1_files/table_representation.png" alt="" width="600px"/>

<br>

## DBMS
- data can be distributed, and have multiple copies
    - DBMS handles merging for retrieving, and backing up
- DBMS also handles security
- need client tool (front end) to access DBMS server
- brands
    - Oracle - the best, expensive
    - MS-SQL server - fully integrates with Windows active directory domain (no need for separate login after logging into windows)
    - mySQL
    - postgreSQL
    - SQLite - not scalable
    - MS access - not robust, used in small applications

<br>

## database design

<br>

### logical design
- use diagrams to layout the database

<br>

### physical design
- implement logical design in a DBMS
- some things you wanted in logical design might not be possible with a certain DBMS

<br>

## SQL history
- relational, data tables are related by certain attributes
- SQL is a declarative language (not procedural)
    - all you need is to tell DBMS WHAT to do, not step by step HOW to do it

<br>

## mySQL pros/cons
- pros
    - free, based on MS-SQL, large support base, works on several OS
- cons
    - not scalable (if run out of space on one server, hard to add on a second server)
    - Oracle bought mySQL, not upgrading mySQL
    - not fully SQL compliant (some MS-SQL functions not found in mySQL)