---
title: "mail的发送"
description: "mail的其他文章：gnu pg、mail的使用"
date: 2020-04-15T21:08:08+08:00
---

使用msmtp实现
============

# installation

## down
```bash
$wget https://marlam.de/msmtp/releases/msmtp-1.6.8.tar.xz
```

## install
```
$xz -d msmtp-1.6.8.tar.xz
$tar xvf msmtp-1.6.8.tar
$cd msmtp-1.6.8
$./configure --prefix=/user/local/msmtp
$make
```

Then install using root
```bash
$make install
```

# configuration
> 需要在user的根目录建立一个新文件：.msmtprc

```bash
$cd ~
$vi .msmtprc
$chmod 600 .msmtprc
```

>>  .msmtprc

```bash
defaults

logfile ~/.msmtp/msmtp.log

# The SMTP server of the provider.
account love_shidie
host smtp.163.com
from love_shidie@163.com
auth login
user love_shidie@163.com
passwordeval gpg  --no-tty -q -d ~/.msmtp/love_shidie-password.gpg

# Set a default account
account default: love_shidie
```

## r+w
```bash
$chmod 600 .msmtprc
```

# Reference
- [gnupg - xiaobin_HLJ80](https://xiaobin80.gitee.io/csdn/post/ops_gnupg/)
