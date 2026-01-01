---
title: "apt的安装问题"
description: "apt安装中使用到卸载、安装指定版本、以及查看deb版本"
date: 2024-09-21T02:20:39+08:00
---
apt适合手动输入(有交互); apt-get适合脚本(无交互).

Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

# Manage

## 卸载
```bash
apt-get autoremove vsftpd --purge
```

## 安装指定版本
```bash
apt-get install <package name>=<version>
```

## 列出版本
```bash
apt-cache madison <<package name>>
```

# DebIan
```
sudo sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list
```
## sudo
sudo.d(directory):
```
Note also, that becasuse sudoers contents can vary widely, no attempt is made to add this 
directive to existing sudoers files on upgrade. Feel free to add the above directive to the 
end of your /etc/sudoers file to enabe this functionality for existing installations if you 
wish! Sudo versions older than the one in Debian 11(bullseye) require the directive will only 
support the old syntax #includedir, and the current sudo will happily accept both @includedir 
and #includedir
```
### install
using root:
```bash
su - root
```
install:
```sudo
apt install sudo
```
### manual
```bash
chmod 640 /etc/sudoers
nano /etc/sudoers
```
在配置文件/etc/sudoers加入：
```bash
# User privilege specification
tdtc ALL=(ALL:ALL) ALL
```
把文件权限改为只读:
```bash
chmod 440 /etc/sudoers
```

## CD-ROM in source
```bash
sudo vi /etc/apt/sources.list
```
注释掉"deb cdrom:xxx"
```bash
##deb cdrom:xxx
```
- no sources file
```
$ sudo cp /usr/share/doc/apt/examples/sources.list /etc/apt/sources.list

$ sudo apt update
```
### Command
```
sudo sed -i '/cdrom/d' /etc/apt/sources.list
```

# Ref
- [Debian中安装使用sudo命令](https://blog.csdn.net/putong11/article/details/9568685)
- [Debian取消从光盘安装软件的方式](https://www.cnblogs.com/yaos/p/6941144.html)
- [media change: please insert the disc labeled](https://askubuntu.com/questions/386265/media-change-please-insert-the-disc-labeled-when-trying-to-install-ruby-on-ra)
