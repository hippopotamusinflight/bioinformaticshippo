---
title: "MySQL Universalclass Lesson3"
linktitle: "MySQL Universalclass Lesson3"
summary:
date: 2020-07-15T13:22:29-04:00
lastmod: 2020-07-15T13:22:29-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 30
menu:
  mySQL_tutorials:
    name: "Lesson3 db design"
    parent: "MySQL Universalclass"
    weight: 2020071513
---

[https://www.youtube.com/watch?v=5z4IUrCAv4A&t=1s](https://www.youtube.com/watch?v=5z4IUrCAv4A&t=1s)

<br>

## 1 database design

<br>

### logical design
- general layout

<br>

### physical design
- which DBMS to use
- constraints of DBMS, security features

<br>

***
## 2 keywords

<br>

### schema
- everything making up a database
- tables, views, roles, permissions, indexes, etc

<br>

### entity = tables
- attribute = column
- tuple/entry/record = row

<br>

### keys
- primary key = attribute that uniquely identifies a record
- candidate key = attribute that could be selected as a primary key
- composite key = 2 or more attributes making up a primary key
- foreign key = attribute providing relationship (links) one table to another table
<br><br>
- alternate/ secondary key = candidate key not chosen to be primary key

<br>

***
## 3 referential integrity
<img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson3_files/ERD2.png" alt="" width="500px"/>  

- underlined = primary key
- dotted underline = foreign key

<br>

- referential integrity = all foreign keys must have corresponding attribute in related table
    - shown in ER diagrams with linking arrows
    - "whenever I reference something in table2, it better exist in table2"

<br>

***
## 4 db diagrams
<img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson3_files/database_diagrams.png" alt="" width="600px"/>  

- IDEF1X and Crow's Foot will be used here

<br>

***
## 5 ERD
- MS visio (expensive)
- Dia (free) http://dia-installer.de/
- Lucidchart (website, education edition) https://www.lucidchart.com

<br>

***
## 6 notations 

<br>

### standard (SN)
- entity (table) name listed in capital letters
- attributes (colnames) listed in parenthesis
- primary key underlined
- foreign key dotted underlined
- con: does not have arrows like ERDs

<br>

### db design language (DBDL)
- alternate to SN
- primary key (PK), secondary key (SK), alternate key (AK), foreign key (FK), listed underneath entity listing
```
e.g. 
  STUDENT (student_id, firstname, lastname, program, email)
      PK student_id
      AK email
  CLASS (class_id, name, day, building_id, begintime, endtime)
      PK class_id
      FK building_id -> BUILDING       # building_id is a foreign key
  STUDENT_CLASS (student_id, class_id)
      PK student_id, class_id
  BUILDING (building_id, name, address, city, zip)
      PK building_id
```

<br>

***
## 7 cardinality
- how tables are related
    - how many attributes in one table can be linked to attributes to another table
- 1:1, 1:N, N:N
- try to avoid N:N relationship

<br>

### crow's foot notation showing cardinality
```
  0 or 1              -----------0-1-|
  exactly 1           -----------1-1-|
  0 or many           -----------0-<-|
  at least 1 or many  -----------1-<-|
```

<br>

***
## 8 normalization
- progressively modify db table
- reduce redundancy and problems
- prevent update anomalies
    - after db made, user can't insert certain records
- more normalized =
    - less problems
    - more tables with less attributes
    - more processing power needed
    - tables more refined

<br>

### normal form
- 3NF is what we want eventually (least repetitive)
- normalization is progressive, so need 1NF to do 2NF

<br>

#### 1st normal form (1NF)
- all records have same number of attributes
- no repeating values
- e.g. PET_OWNER (ID, Name, Pet1, Pet2, Pet3, Pet4, Pet5)
    - this creates a artificial limit - cannot have > 5 pets
    - wastes space - having < 5 pets, rest will be NULL
    - (**drawbacks of 1NF**)

<br>

#### 2nd normal form (2NF)
- primary key defined
- can be composite (2+ columns)
- primary key determines all other attributes
<img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson3_files/NF2.png" alt="" width="450px"/>

<br>

#### 3rd normal form (3NF)
- all non-key attributes depend on primary key,
- and NOT on other non-key attributes
- e.g. table in 2NF but NOT in 3NF  
<img src="/tutorials//mySQL_tutorials/mySQL_universalclass_lesson3_files/NF2_not_NF3.png" alt="" width="450px"/>  
    - "manager_name" and "manager_title" does NOT depend on primary key (ID),
    - instead depend on non-key attribute "manager_id"

##### making it 3NF
- split EMPLOYEE table into 3 tables
```
EMPLOYEE (employee_id, name, address)
    PK employee_id
MANAGER (manager_id, name, title)
    PK manager_id
EMPLOYEE_MANAGER (employee_id, manager_id)  <-- linking table, need a composite key
    PK employee_id, manager_id
    FK employee_id -> EMPLOYEE
    FK manager_id -> MANAGER
```

<br>

#### example of 1NF vs 2NF vs 3NF
- school keeping track of students enrolled, classes offered

<br>

##### 1NF design
- in STUDENT table: each student is stored in 1 row
- in CLASS table: each class is stored in 1 row  
<img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson3_files/NF_example.png" alt="" width="400px"/>  

- each student limited to 3 classes
- question = how to correctly relate STUDENT to CLASS?
- solution = break up STUDENT

<br>

##### 3NF design
<img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson3_files/NF3_example.png" alt="" width="450px"/>  

- STUDENT table strictly only have info about student
- CLASS table strictly only have info about classes
- make a third table STUDENT_CLASS to relate STUDENT to CLASS
- e.g. STUDENT_CLASS table
    - student_id = 20	---> STUDENT table to look up student name
    - class_id = 3001 ---> CLASS table to get class name
- can now have students taking any number of classes
    - (multiple class_id entries for same student_id entry in STUDENT_CLASS table)
- and students can take no class
    - (won't be in STUDENT_CLASS table)

##### cardinality
```
STUDENT-|-|----0-< STUDENT_CLASS >-0------|-|-CLASS
       
       1:N relationship btw STUDENT and STUDENT_CLASS
       1:N relationship btw CLASS and STUDENT_CLASS
       
in 1NF previous, had N:N (many students to many classes) cardinality, not good design
in 3NF, now have 1 student to many classes, better design
```

<br>

#### 4th normal form (4NF)
- any record should not have > 1 related attribute
- theoretical best, but causes too many lookups and slow response time
- have to de-normalize into 3NF
- e.g. of 4NF: EMPLOYEE table
```
  EMPLOYEE_NAME       (emp_id, name)
  EMPLOYEE_ADDRESS    (emp_id, address)	
  EMPLOYEE_TELEPHONE  (emp_id, telephone)
  EMPLOYEE_SALARY     (emp_id, salary)
```
- to extract a employee's name, address, telephone, salary into a single output 
    - will require 4 different lookups