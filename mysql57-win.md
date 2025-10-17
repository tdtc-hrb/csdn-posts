---
title: "安装配置MySQL 5.7"
description: "Windows绿色安装"
date: 2024-05-07T12:08:39+08:00
---

# 一、配置和安装
ini配置详见附录

## 预配置
  需要[vc12 redist](https://docs.microsoft.com/en-US/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2013-vc-120)

### 设置环境变量
```bash
名称：MYSQL57_HOME
赋值：D:\progFiles\mysql-5.7
```

### init data
```bash
%MYSQL57_HOME%\bin\mysqld --initialize-insecure --console
```

## 注册/注销服务

### 注册
多个版本的MySQL运行：
```bash
%MYSQL57_HOME%\bin\mysqld --install MySQL57 --defaults-file="D:\progFiles\mysql-5.7\my.ini"
```

### 注销
```bash
sc delete mysql57
```

## warnning info
```
2024-05-13T16:33:17.643640Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2024-05-13T16:33:17.644951Z 0 [ERROR] Cannot open Windows EventLog; check privileges, or start server with --log_syslog=0
2024-05-13T16:33:17.823377Z 0 [Warning] InnoDB: New log files created, LSN=45790
2024-05-13T16:33:17.859294Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2024-05-13T16:33:17.936952Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 80e0a1b1-1146-11ef-a356-00be430eb090.
2024-05-13T16:33:17.940739Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2024-05-13T16:33:18.666030Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
2024-05-13T16:33:18.666152Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
2024-05-13T16:33:18.668880Z 0 [Warning] CA certificate ca.pem is self signed.
2024-05-13T16:33:18.835389Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
```

注意： 可以忽略如下警告信息：
```bash
CA certificate ca.pem is self signed.
```

### [Connection TLS Protocol Configuration](https://dev.mysql.com/doc/refman/5.7/en/encrypted-connection-protocols-ciphers.html#encrypted-connection-protocol-configuration)
```sql
mysql> SHOW GLOBAL VARIABLES LIKE 'tls_version';
```
To change the value of tls_version, set it at server startup. 
For example, to permit connections that use the TLSv1.1 or TLSv1.2 protocol, 
but prohibit connections that use the less-secure TLSv1 protocol, use these lines in the server my.cnf(my.ini - windows) file:
```
# only support v1.2
tls_version=TLSv1.2
```

### [configuration CA](https://dev.mysql.com/doc/mysql-security-excerpt/5.7/en/using-encrypted-connections.html#using-encrypted-connections-server-side-startup-configuration)
```
# enable the server for encrypted connections
ssl_ca=ca.pem
ssl_cert=server-cert.pem
ssl_key=server-key.pem
```



# 二、使用mysqladmin和mysql

## 1．修改密码
> 格式：mysqladmin -u用户名 -p旧密码 password 新密码

### 1) 空密码
> 注：如果root没有密码，-p旧密码, 可以省略

```bash
%MYSQL57_HOME%\bin\mysqladmin -uroot password qazxsw
```

### 2) 有密码
```bash
%MYSQL57_HOME%\bin\mysqladmin -uroot -pqazxsw password mypassword123456
```

## 2. 连接mysql
> 格式： mysql -h主机地址 -u用户名 －p用户密码

### 1) 本机
```bash
%MYSQL57_HOME%\bin\mysql -uroot -p
```

### 2) 远程主机
```bash
%MYSQL57_HOME%\bin\mysql -h192.168.2.101 -uroot -pqazxsw
```

# 附：my.ini
```bash
[mysqld]
innodb_buffer_pool_size = 1012M

# These are commonly set, remove the # and set as required.
basedir = D:/progFiles/mysql-5.7/
datadir = D:/progFiles/mysql-5.7/data
port = 3306

character-set-server=utf8mb4
# only support v1.2
tls_version=TLSv1.2
[client]
port=3306
plugin-dir=D:/progFiles/mysql-5.7/lib/plugin
```

# 参考文档
- [2.10.1 Initializing the Data Directory](https://dev.mysql.com/doc/refman/5.7/en/data-directory-initialization.html)
- [Enable SSL Connections for MySQL](https://techdocs.broadcom.com/us/en/ca-enterprise-software/layer7-api-management/api-gateway/10-0/install-configure-upgrade/enable-ssl-connections-for-mysql.html)
