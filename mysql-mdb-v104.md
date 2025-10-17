---
title: "安装 MariaDB 10.3/10.4(MySQL 5.7)数据库"
description:  "OS: CentOS 7"
date: 2023-08-06T01:08:39+08:00
---

- OS    
CentOS v7.9(2009)
- [MariaDB](https://mariadb.com/kb/en/mariadb-vs-mysql-compatibility/)    
|MariaDB|MySQL|
|-|-|
|v10.2/3/4 | v5.7|
|v10.0/1   | v5.6|
|v5.5      | v5.5|


# 1. 准备安装
> 卸载旧版本Maria

## 1) 查询
```bash
[tdtc@localhost ~]$ sudo rpm -qa | grep mariadb
[sudo] password for tdtc:
mariadb-libs-5.5.68-1.el7.x86_64
```

## 2) uninstall
```bash
sudo yum -y remove mariadb*
```

# 2. install

## Step 1: [Add MariaDB Yum Repositories](https://downloads.mariadb.org/mariadb/repositories)
/etc/yum.repos.d/mariadb.repo:
```bash
# MariaDB 10.4 CentOS repository list - created 2023-08-26 09:24 UTC
# https://mariadb.org/download/
[mariadb]
name = MariaDB
# rpm.mariadb.org is a dynamic mirror if your preferred mirror goes offline. See https://mariadb.org/mirrorbits/ for details.
# baseurl = https://rpm.mariadb.org/10.4/centos/$releasever/$basearch
# baseurl = https://mirrors.neusoft.edu.cn/mariadb/yum/10.4/centos/$releasever/$basearch ## neusoft
## aliyun
baseurl = https://mirrors.aliyun.com/mariadb/yum/10.4/centos/$releasever/$basearch
module_hotfixes = 1
# gpgkey = https://rpm.mariadb.org/RPM-GPG-KEY-MariaDB
gpgkey = https://mirrors.aliyun.com/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck = 1
```

## Step 2: Install MariaDB Server
```bash
sudo yum install MariaDB-server MariaDB-client
```

- run
```bash
sudo systemctl start mysql.service
```

## Step 3: Secure MariaDB Install
> Default: Enter

```bash
[tdtc@localhost ~]$ sudo /usr/bin/mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.
Enter current password for root (enter for none):
OK, successfully used password, moving on...
Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.
Set root password? [Y/n]
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!
By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.
Remove anonymous users? [Y/n]
 ... Success!
Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? [Y/n]
 ... Success!
By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.
Remove test database and access to it? [Y/n]
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!
Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.
Reload privilege tables now? [Y/n]
 ... Success!
Cleaning up...
All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Step 4: Working with MariaDB

```bash
[tdtc@localhost ~]$ sudo mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 21
Server version: 10.4.31-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

# 3. Open port
- see state - firewall
```bash
sudo firewall-cmd --state
```

- open 3306
```bash
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
```

- reload
```bash
sudo firewall-cmd --reload
```
- verification
```bash
sudo firewall-cmd --zone=public --list-ports
```

# Reference
- [CentOS 7.4 如何安装 MariaDB 10.3.9 Stable 数据库](https://www.cnblogs.com/hglibin/p/9567367.html)
- [How to Install MariaDB 10.4 on CentOS/RHEL 7/6](https://tecadmin.net/install-mariadb-10-centos-redhat/)
