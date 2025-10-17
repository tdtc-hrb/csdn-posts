---
title: "安装MySQL 8.0"
date: 2024-12-26T01:08:39+08:00
---

# MySQL 5 & 8
- Windows 建议使用 包(msi)安装 MySQL v8.0
- [安装MySQL 5.7](https://tdtc-hrb.github.io/csdn/post/mysql57-win/)
## remote server
```
bind-address=0.0.0.0
```

## utf8
```sql
[MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3,
but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
```

- mysql5
```sql
character-set-server=utf8
```
- mysql8
```sql
character-set-server=utf8mb4
```

## validate password
注意： [mysql8.0.4](https://mysqlserverteam.com/mysql-8-0-4-new-default-authentication-plugin-caching_sha2_password/)，
我们将无法直接使用phpMyAdmin来登录！

使用phpMyAdmin需要设置MySQL5.7兼容模式：
```
authentication_policy=mysql_native_password,,
```

- mysql5
```sql
mysql_native_password
```
- mysql8
```sql
caching_sha2_password
```


# Ubuntu
install mysql8:
```
sudo apt install mysql-server
```

```
sudo systemctl start mysql.service
```

## config mysql secure
```
sudo mysql
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'xbfirst80';
```

```
sudo mysql_secure_installation
```

## add user
password rule:
```
SHOW VARIABLES LIKE 'validate_password%';
```
DBA:
```sql
CREATE USER 'DBAdmin'@'localhost'' IDENTIFIED BY 'xb80first';
GRANT ALL PRIVILEGES ON *.* TO 'DBAdmin'@'localhost'' WITH GRANT OPTION;
flush privileges;
```
remote user:
```sql
CREATE USER 'tdtc2022'@'%' IDENTIFIED BY 'qaz1xsw2';
GRANT SELECT,INSERT,UPDATE,DELETE ON carnumber.* TO 'tdtc2022'@'%';
flush privileges;
```


# 参考文档
- [MySQL installation on Ubuntu 20.04 error when using mysql_secure_installation](https://stackoverflow.com/a/72115499)
- [Failed! Error: SET PASSWORD has no significance for user ‘root’@’localhost’ as the authentication method used doesn’t store authentication data in the MySQL server](https://exerror.com/failed-error-set-password-has-no-significance-for-user-rootlocalhost-as-the-authentication-method-used-doesnt-store-authentication-data-in-the-mysql-server/)
- [How To Install MySQL on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)
- [Mysql adding user for remote access](https://stackoverflow.com/a/16288118)
