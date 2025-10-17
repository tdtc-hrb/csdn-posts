---
title: "ssh相互免密"
description: "使用ssh-copy-id方式"
date: 2020-04-13T11:38:08+08:00
---
> OS: CentOS 7

|Ip	|/etc/hostname|
|  -|   -         |
|192.168.0.110	|host1|
|192.168.0.111	|host2|

> Note:
>> On every host, run following commands, Use xiaobin112(user) to do all following.

# Prepare
Establish Directory ".ssh"

```bash
$mkdir ~/.ssh
$chmod 700 ~/.ssh
```

# Stage I

## 1. config
> 另外一种方式为：把所有机器的IP和hostname放在/etc/hosts里，在此不再赘述, 请参见《[ZooKeeper multi-server install](https://xiaobin80.gitee.io/csdn/post/ops_zookeeper/) - 3.FQDN》。

```bash
$vi ~/.ssh/config
```

```bash
Host host1
        HostName 192.168.0.110
        Port 22
        User xiaobin112

Host host2
        HostName 192.168.0.111
        Port 22
        User xiaobin112
```

## 2. r+w
```bash
$chmod 600 ~/.ssh/config
```

# Stage II

## 1. gen
```bash
(Just press ENTER for all input values)
ssh-keygen -t rsa -P ''
```

## 2. file copy
> host1 and host2:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub xiaobin112@host1
ssh-copy-id -i ~/.ssh/id_rsa.pub xiaobin112@host2
```

# Stage III
## 1. test

```bash
$ssh xiaobin112@host1
$ssh xiaobin112@host2
```
