---
title: "MySQL Universalclass Lesson2"
linktitle: "MySQL Universalclass Lesson2"
summary:
date: 2020-07-15T11:27:43-04:00
lastmod: 2020-07-15T11:27:43-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 30
menu:
  mySQL_tutorials:
    name: "Lesson2 install"
    parent: "MySQL Universalclass"
    weight: 2020071511
---

[https://www.youtube.com/watch?v=pdNGcrtJdb8&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO&index=2](https://www.youtube.com/watch?v=pdNGcrtJdb8&list=PLBlpUqEneF0-xZ1ctyLVqhwJyoQsyfOsO&index=2)

<br>

## installing mySQL (Windows, Linux, macOSX)

<br>

### Windows
- mysql.com
- mysql community edition (GPL) (mysql community server 5.6.26)
- computer needs .net framework 4.0+

<br>

#### MySQL installer

##### setup type
- developer default
```
  mysql server            <-- essential
  mysql workbench         <-- essential
  mysql for excel 1.3.4   <-- need pre-installed
  mysql for visual studio <-- need pre-installed
  mysql connectors/ python
  mysql fabric 1.5.4
  (also installs sample database for practice)
```

<br>

##### type and networking
```
  config type		    development machine
  connectivity	    TCP/IP
  port number		    3306
  open firewall     port for network acccess (yes)
  named pipe        (unselected)
  shared memory     (unselected)
```

<br>

##### accounts and roles
- mysql root password

<br>

##### mysql user accounts
- create later in mySQLworkbench

<br>

##### windows service
- can run mySQL server in the background as a windows service
```
  configure mysql server    as windows service
  windows service name      mysql56
  run windows service as    standard account
```

<br>

##### apply server configuration
- execute
- will generate some config files

<br>

##### connect to server
- server running on local computer
- enter username and password to connect to server
- `check` to verify connection

<br>

##### apply server configuration
- finishes configuration

<br>

***
### Linux-Ubuntu (9:20)
- Ubuntu software center > search "mysql""

<br>

***
### macOSX (13:39)
- need to download server and workbench separately

<br>

#### mysql server
- mysql community server 5.6.26 (.dmg or .tar)
- install with default settings
- start/stop server
    - system preference > MySQL  
    <img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson2_files/start_mysql_server.png" alt="" width="500px"/>
    <img src="/tutorials/mySQL_tutorials/mySQL_universalclass_lesson2_files/mysql_server_config.png" alt="" width="500px"/>  

<br>

#### mysql workbench
- download, install, open

<br>

##### setup new connection
- MySQL Connections (+)
```
	connection name      local
	connection method    standard(TCP/IP)
	hostname             127.0.0.1
	port                 3306
	username             root
	password             store in keychain
	default schema       blank
	
	test connection
```

##### open SQL editor
- double click on "local" server connection
- under "Query 1"
    - `CREATE database brian;`
    - inserted brian schema under SCHEMAS panel

<br>

##### mysql program file structure
```
/usr/local/mysql/
    bin    scripts
    data   schemas    does not have permission, only DBMS has access	
```

<br>

##### mysql folder permissions
```
minghan:mysql$ ls -lA
total 872
-rw-r--r--   1 _mysql  _mysql  335809 13 Apr 07:46 LICENSE
-rw-r--r--   1 _mysql  _mysql  101807 13 Apr 07:46 LICENSE.router
-rw-r--r--   1 _mysql  _mysql     687 13 Apr 07:46 README
-rw-r--r--   1 _mysql  _mysql     700 13 Apr 07:46 README.router
drwxr-xr-x  35 _mysql  _mysql    1120 13 Apr 08:34 bin
drwxr-x---  28 _mysql  _mysql     896  7 Jul 17:08 data
drwxr-xr-x   6 _mysql  _mysql     192 13 Apr 08:32 docs
drwxr-xr-x  15 _mysql  _mysql     480 13 Apr 08:32 include
drwxr-x---   3 _mysql  _mysql      96  5 Jul 20:26 keyring
drwxr-xr-x  21 _mysql  _mysql     672  5 Jul 20:25 lib
drwxr-xr-x   4 _mysql  _mysql     128 13 Apr 08:32 man
drwxrwxr-x   2 _mysql  _mysql      64 13 Apr 08:32 run
drwxr-xr-x  33 _mysql  _mysql    1056 13 Apr 08:32 share
drwxr-xr-x   5 _mysql  _mysql     160 13 Apr 08:32 support-files
drwxr-xr-x   3 _mysql  _mysql      96 13 Apr 08:32 var
```