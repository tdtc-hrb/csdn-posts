---
title: "MySQL 主从复制"
description: "本系列包括主备切换 - MySQL+MHA"
date: 2022-04-13T09:38:08+08:00
---
> 在进行配置前，请确认已经安装 [mysql5.7 - CentOS](https://tdtc-hrb.github.io/csdn/post/mysql57-cenos7/)

# 1. 网络配置
Replication Group 1:    

|Name|IP|Memo|
|  - | -| -  |
|mysql1|192.168.0.160|Master|
|mysql11|192.168.0.161|Slave|
|mysql12|192.168.0.162|Slave|

Replication Group 2(Maria DB):    

|Name|IP|Memo|
|  - | -| -  |
|mysql2|192.168.0.170|Master|
|mysql21|192.168.0.171|Slave|
|mysql22|192.168.0.172|Slave|

# 2. Configure MySQL cluster
## 1) Master Server
[Setting the Replication Master Configuration](https://dev.mysql.com/doc/refman/5.7/en/replication-howto-masterbaseconfig.html)

> /etc/my.cnf
```bash
[mysqld]
log-bin=mysql160-bin
server-id=160
```

## 2) Slave Server
[Setting the Replication Slave Configuration](https://dev.mysql.com/doc/refman/5.7/en/replication-setup-slaves.html)
> /etc/my.cnf

- Slave 1
```bash
[mysqld]
server-id=161
```

- Slave 2
```bash
[mysqld]
server-id=162
```

# 3. replication user
> Master Server
[Creating a User for Replication](https://dev.mysql.com/doc/refman/5.7/en/replication-howto-repuser.html)

```sql
mysql> CREATE USER 'repl'@'192.168.0.%' IDENTIFIED BY 'qaz1!Xsw2@';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.0.%';
```

# 4. 通信
> Slave server

## 1) replication threat 参数
[Setting the Master Configuration on the Slave](https://dev.mysql.com/doc/refman/5.7/en/replication-setup-slaves.html)


### Master IP
```bash
192.168.0.160
```

### Master User
```bash
repl
```

### Master Password
```bash
qaz1!Xsw2@
```

### log info
> Master Server

[Obtaining the Replication Master Binary Log Coordinates](https://dev.mysql.com/doc/refman/5.7/en/replication-howto-masterstatus.html)

```sql
mysql > SHOW MASTER STATUS;
```
![sql status](https://github.com/tdtc-hrb/csdn/raw/master/images/20190227204027156.png)

## 2) 赋值
在Savle 1与2中执行。

```sql
mysql > CHANGE MASTER TO MASTER_HOST='192.168.0.160', \
MASTER_USER='repl', \
MASTER_PASSWORD='qaz1!Xsw2@', \
MASTER_LOG_FILE='mysql160-bin.000002', \
MASTER_LOG_POS=154;
```

### [Start the slave threads](https://dev.mysql.com/doc/refman/5.7/en/replication-setup-slaves.html)

```sql
mysql> START SLAVE;
```

## 3) 改参数（如果需要重新设定）

### stop slave thread
```sql
mysql> STOP SLAVE;
```

### exec
执行 "2) 赋值"

### start slave thread
```sql
mysql> START SLAVE;
```

# 5. test data

## 1) create db
```sql
create database blog;
```

## 2) import data

[putty](https://putty.org/)(pscp):

```cmd
pscp E:\users.sql tdtc@192.168.0.160:/home/tdtc/
pscp E:\articles.sql tdtc@192.168.0.160:/home/tdtc/
pscp E:\ci_sessions.sql tdtc@192.168.0.160:/home/tdtc/
pscp E:\pages.sql tdtc@192.168.0.160:/home/tdtc/
```
[导入导出数据库](https://xiaobin80.gitee.io/csdn/post/mysql-imp-exp/)

```sql
mysql>CREATE DATABASE blog;
mysql>USE blog;
mysql>SOURCE /home/tdtc/users.sql;
mysql>SOURCE /home/tdtc/articles.sql;
mysql>SOURCE /home/tdtc/ci_sessions.sql;
mysql>SOURCE /home/tdtc/pages.sql;
```

# 参考文档
- [CentOs7.3 搭建 MySQL 5.7.19 主从复制，以及复制实现细节分析](https://cloud.tencent.com/developer/article/1041222)
