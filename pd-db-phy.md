---
title: "powerDesigner生成phy图"
date: 2022-09-10T02:26:39+08:00
---
> 使用版本：v16.1

# 一、连接数据库

## 1. 新建pdm
File-》New Model    
model types -> physical data model
![new project](https://github.com/tdtc-hrb/csdn/raw/master/images/2019082714153526.png)

> model name: newmodel-phy    
dbms: mysql5.0

## 2. config connect profile
DataBase -> Configure connections    
Connection profile
![Connection profile](https://github.com/tdtc-hrb/csdn/raw/master/images/20190827152244711.png)

## 3. connect
Database -> connect
![connect dialog](https://github.com/tdtc-hrb/csdn/raw/master/images/20190827152618753.png)

Flag: "Disconnect" Available

- dcp文件（mysql5-crm.dcp）
```file
[ConnectionProfile]
ConnectionType=JDBC
Dbms=MySQL
DisableBind=0
Driver=com.mysql.jdbc.Driver
JarFiles=D:\war\mysql-connector-java-5.1.49.jar
LogId=root
TrimSpaces=0
URL=jdbc:mysql://106.14.141.40:3306/crm
```

# 二、生成phy图

## 1. Reverse Engineer
File -> Reverse Engineer->Database    
![Reverse Engineer 1](https://github.com/tdtc-hrb/csdn/raw/master/images/20190827153110808.png)
![Reverse Engineer 2](https://github.com/tdtc-hrb/csdn/raw/master/images/20190827153305897.png)
选择数据库：
![Reverse Engineer 3](https://github.com/tdtc-hrb/csdn/raw/master/images/20190827153501155.png)
其他数据库全部反选：    
例如：    
![Reverse Engineer 4](https://github.com/tdtc-hrb/csdn/raw/master/images/20190827153501155.png)
最后，点击ok生成phy图。

> Tips：如果想从pdm生成CDM，Tools -> Generate Conceptual Data Model
