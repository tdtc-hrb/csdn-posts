---
title: "MySQL导入导出数据库"
date: 2022-09-10T02:18:39+08:00
---

# 导出数据库
```bash
$mysqldump dbName > dbName.sql -u user -p
```

- v5.6/5.7
```bash
$mysqldump --no-defaults dbName > dbName.sql -u user -p
```

# 导入数据库
```bash
mysql>CREATE DATABASE `dbName` CHARACTER SET utf8 COLLATE utf8_general_ci;

mysql>USE dbName;
```

- win
```cmd
mysql>SOURCE C:/Users/xiaobin/dbName.sql;
```

- Linux
```
mysql>SOURCE /home/xiaobin/dbName.sql;
```

## upload
> win10
```cmd
scp D:\progFiles\carnumber.sql tdtc@192.168.0.151:/home/tdtc/
```

## create user
```bash
mysql> CREATE USER 'DBAdmin'@'localhost' IDENTIFIED BY 'xbfirst';
```

```bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'DBAdmin'@'localhost' WITH GRANT OPTION;
mysql> flush privileges;
```


# Reference
- [MySQL Create Database with UTF8 Character Set Syntax](https://www.euperia.com/development/mysql-create-database-with-utf8-character-set-syntax/1064)
