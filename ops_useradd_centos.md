---
title: "增加用户"
description: "CentOS"
date: 2020-04-15T00:18:08+08:00
---
> ali云服务器

# 1. User Add and Set password

## 1) add user
```bash
useradd	site_user
```

# 2) set passwd
```bash
passwd -x 180 site_user
```

# 3) set home_dir
```bash
usermod -d /home/fdn site_user
```

# 2. assigning permissions
> 如果多个目录需要设置，则多次重复"(2)设置"即可。

> 文件系统是否支持acl
```bash
df -T | awk '{print $1,$2,$NF}' | grep "^/dev"
grep -i acl /boot/config*
```

> 分区是否支持acl
>> 注意：在ali服务器上是vda1。
```bash
tune2fs -l /dev/vda1 | grep acl
```

## 1) 查看
> 目录的acl设置情况。

```bash
getfacl	/home/fdn
```

## 2) 设置
> 设置指定目录的acl。
```bash
setfacl -m user:site_user:rwx /home/fdn
```

# 参考文章
- 《[在 Linux 上给用户赋予指定目录的读写权限](https://linux.cn/article-8487-1.html)》
- 《[setfacl命令](http://man.linuxde.net/setfacl)》
- [Unix / Linux change a user's home directory - usermod](http://www.spiration.co.uk/post/1294/Unix%20/%20Linux%20change%20a%20user%27s%20home%20directory%20-%20usermod)
- [give read write access to directory in linux](https://www.tecmint.com/give-read-write-access-to-directory-in-linux/)
