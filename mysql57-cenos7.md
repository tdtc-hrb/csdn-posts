---
title: "CentOS7/8安装mysql5.7"
date: 2022-09-24T06:21:39+08:00
---
> 本安装大部分同["安装 MariaDB 10.3数据库"](https://tdtc-hrb.github.io/csdn/post/mysql-mdb-v103/)

# Installation

## 1. add repo
```bash
sudo yum install http://repo.mysql.com/mysql57-community-release-el7.rpm
```

## 2. install
import GPG:
```
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```
## CentOS7
```bash
sudo yum install mysql-community-server
```

## CentOS8
Install after doing the following(disable default repo, config-manager, close gpgcheck):
```bash
sudo dnf install mysql-community-server
```

### disable default repo
```bash
sudo dnf remove @mysql
sudo dnf module reset mysql
sudo dnf module disable mysql
```

### config-manager
```bash
sudo dnf config-manager --disable mysql80-community
sudo dnf config-manager --enable mysql57-community
```

### close gpgcheck
```bash
sudo vi /etc/yum.repos.d/mysql-community.repo
```
mysql-community.repo:
```
gpgcheck=0
```


## 3. init
- start
```bash
sudo systemctl start mysqld.service
```

- login
```bash
sudo grep "temporary password" /var/log/mysqld.log
```
```bash
mysql -uroot -p
```

- change policy

- change root password
```bash
mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('xbfirst80');
```

### change policy
```bash
mysql> SHOW VARIABLES LIKE 'validate_password%';
```
```bash
mysql> SET GLOBAL validate_password_length = 6;
```
```bash
mysql> set global validate_password_policy=0;
```


# Reference
- [A Quick Guide to Using the MySQL Yum Repository](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)
- [Install MySQL 5.7 on CentOS 8 / RHEL 8 Linux](https://computingforgeeks.com/install-mysql-5-7-on-centos-rhel-linux/)
- [GPG keys issue while installing mysql-community-server](https://stackoverflow.com/a/71265327)
