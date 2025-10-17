---
title: "在阿里云上部署pure-ftpd"
description: "ops系列-云服务器(centos7)"
date: 2020-04-13T00:08:08+08:00
---

> Server：ali云服务器ECS    
OS: CentOS 7.2 x64

# 1. 管理防火墙
增加ftp端口    
![2017-10-11 17:18](https://gitee.com/xiaobin80/csdn/raw/master/images/20171011171838468.png)

# 2. 其他系统管理

##  1) 设置SeLlinux
```bash
# setsebool -P ftp_home_dir on
```

##  2) 设置user
```bash
# useradd -m fdn -s /sbin/nologin
# passwd fdn
```


# 3. 安装pure-ftpd
```bash
# yum install pure-ftpd
```

##  1) Setup
使用root简单设置pure-ftpd.conf

```bash
# vi /etc/pure-ftpd/pure-ftpd.conf
```

```bash
# If you want simple Unix (/etc/passwd) authentication, uncomment this
UnixAuthentication            yes

# Cage in every user in his home directory
ChrootEveryone              yes

# If you want to log all client commands, set this to "yes".
# This directive can be duplicated to also log server responses.
VerboseLog                  yes

# Disallow anonymous connections. Only allow authenticated users.
NoAnonymous                 yes
```

## 2) run
```bash
# chkconfig pure-ftpd on
# service pure-ftpd start
```


# 参考文档
- [How To Install Pure-FTPd on CentOS 6 Via Yum](http://www.lifelinux.com/how-to-install-pure-ftpd-on-centos-6-via-yum/)
- [Setup FTP server on centos 7](http://www.krizna.com/centos/setup-ftp-server-centos-7-vsftp/)
