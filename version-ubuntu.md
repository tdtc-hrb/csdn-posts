---
title: "查看版本-Ubuntu"
date: 2024-03-01T02:12:39+08:00
tags: ["Glibc", "sh", "binutils", "system"]
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

# components version
查看内核版本:
```bash
#uname -a
```

## 查看Glibc库版本
```bash
#apt-cache show libc6
```

## 查看binutils版本
```bash
#ld -v
```

# 查看ubuntu版本号
```bash
#cat /etc/issue
```

## 查看debian对应版本
```
#cat /etc/debian_version
```

### [debian code name](https://www.debian.org/releases/)
|code|version|status|
|-|-|-|
|bullseye|11|statble|
|buster|10|old statble|
|stretch|9|old old statble|
|jessie|8|archived|
|wheezy|7|obsolete|
|squeeze|6.0|obsolete|
|lenny|5.0|obsolete|
|etch|4.0|obsolete|
|sarge|3.1|obsolete|
|woody|3.0|obsolete|
|potato|2.2|obsolete|
|slink|2.1|obsolete|
|hamm|2.0|obsolete|
