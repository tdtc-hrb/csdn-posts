---
title: "CentOS8安装mysql8.0"
date: 2022-06-27T03:21:39+08:00
tags: ["Rocky Linux 8", "Alma Linux 8", "Percona8"]
---

### Installing and Setting up the Percona Server for MySQL 8.0  Repository
```
yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm
```
disable mysql module repo:
```
percona-release setup ps80
```

### Installing and Setting up the Percona Server for MySQL 8.0 Binaries
```
yum -y install percona-server-server
```
Launch:
```
systemctl start mysqld
systemctl status mysqld
```

## change password
```bash
sudo grep "temporary password" /var/log/mysqld.log
```
```bash
mysql -uroot -p
```

Change default password：
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'qaz1!Xsw2@';
```

### Weak password
```bash
mysql> set global validate_password.policy=0;
```

### set password
```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'xbfirst80';
```


# Ref
- [Installing Percona Server for MySQL on Rocky Linux 8](https://www.percona.com/blog/installing-percona-server-for-mysql-on-rocky-linux-8/)
- [How to Change MySQL Password Policy Level](https://tecadmin.net/change-mysql-password-policy-level/)
