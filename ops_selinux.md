---
title: "SELinux的开启/关闭"
description: "临时和永久的两种方式"
date: 2020-04-15T00:20:08+08:00
---
>  (root权限)

SELinux(Security-Enhanced Linux) 是美国国家安全局（NSA）对于强制访问控制的实现，是 Linux历史上最杰出的新安全子系统。

# 1. 临时
> 临时的意思就是宽容模式(0)强制模式(1)与的切换
> 默认是：强制模式


- 强制模式
```bash
$setenforce 1
```

- 宽容模式
临时关闭
```bash
$setenforce 0
```


# 2. 永久
> /etc/selinux/config

- 开启
```bash
SELINUX=enforcing
```

- 关闭
```bash
SELINUX=disabled
```

# 3. 查看SELinux的工作模式
```bash
getenforce
```
