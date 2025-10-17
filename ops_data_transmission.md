---
title: "Linux与Windows互传数据"
description: "光盘, scp"
date: 2023-02-15T16:08:08+08:00
---

# 一、光盘
制作ISO:    
打开UltralISO，把需要的文件拖拽过去即可。

## 2. Linux
### 1) look
```bash
ls -l /dev | grep cdrom
```

### 2) load
```bash
sudo mount /dev/cdrom /mnt/
```

### 3) unload
```bash
sudo umount /mnt
```

# 二、scp
如果Windows没有SSH服务器，只能进行发送.

## 发送
- Linux
```bash
scp /home/tdtc/go1.19.7.linux-amd64.tar.gz xiaobin@192.168.211.112:/D:\war
```

- Windows    
Windows的scp包含在putty工具中。
```bash
pscp f:\docker-soft\docker-compose-Linux-x86_64 tdtc@192.168.211.131:/home/tdtc/

pscp f:\docker-soft\protobuf-cpp-3.21.12.tar.gz tdtc@192.168.211.131:/home/tdtc/

pscp f:\docker-soft\go1.19.7.linux-amd64.tar.gz tdtc@192.168.211.131:/home/tdtc
```

## 接收
注意：仅适用于win10.

- 安装OpenSSH
![添加功能](https://gitee.com/xiaobin80/csdn/raw/master/images/20200222182445523.png)
开始菜单-》设置-》应用-》管理可选功能

- 开启OpenSSH
![win info](https://gitee.com/xiaobin80/csdn/raw/master/images/2020022218283850.png)
注意：关闭Windows防火墙或允许22端口。
