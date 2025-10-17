---
title: "多服务器ZooKeeper安装"
description: "ZooKeeper multi-server install"
date: 2021-04-14T01:08:08+08:00
---
> JDK install see [设置OpenJDK](https://tdtc-hrb.github.io/csdn/post/ops_openjdk_centos/)


# Stage I
## 1. 《[sudo - CentOS](https://tdtc-hrb.github.io/csdn/post/ops_sudo/)》

## 2. FQDN

```bash
#127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
#::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.0.137 tdtc101
192.168.0.138 tdtc102
192.168.0.139 tdtc103
```

## 3. 《[ssh相互免密](https://tdtc-hrb.github.io/csdn/post/ops_ssh_without_password/)》

# Stage II
## 1. unzip
```bash
$wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.6.3/apache-zookeeper-3.6.3-bin.tar.gz --no-check-certificate
$mkdir -p ~/app/zookeeper
$tar xzvf apache-zookeeper-3.6.3-bin.tar.gz -C ~/app/zookeeper
$mv ~/app/zookeeper/apache-zookeeper-3.6.3-bin ~/app/zookeeper/3.6.3
```

## 2. zk home
```bash
$sudo vi /etc/bashrc
```

```bash
export ZK_HOME=/home/tdtc/app/zookeeper/3.6.3
export PATH=$PATH:$ZK_HOME/bin
```


# Stage III
## 1. zoo.cfg
Add the following configuration, and the other options default.
```bash
dataDir=/home/tdtc/app/zookeeper/data

# server.id=host:port:port
server.1=tdtc101:2888:3888
server.2=tdtc102:2888:3888
server.3=tdtc103:2888:3888
```

### upload
> Note: Windows CMD

```CMD
pscp d:\zk-clu\zoo.cfg tdtc@192.168.0.107:/home/tdtc/app/zookeeper/3.6.3/conf
pscp d:\zk-clu\zoo.cfg tdtc@192.168.0.108:/home/tdtc/app/zookeeper/3.6.3/conf
pscp d:\zk-clu\zoo.cfg tdtc@192.168.0.109:/home/tdtc/app/zookeeper/3.6.3/conf
```

## 2. zk id
> create a file named myid under the data directory

```bash
$cd /home/tdtc/app/zookeeper/data
```

- tdtc101
```bash
1
```
- tdtc102
```bash
2
```
- tdtc103
```bash
3
```

# Stage IV

## 0. fw setup
[soft-list(port) - firewall](https://tdtc-hrb.github.io/csdn/post/ops_firewall.md)

## 1. run
```bash
$zkServer.sh start
```

## 2. status
```bash
$zkServer.sh status
```

# Reference
- [ZooKeeper multi-server config docs](https://zookeeper.apache.org/doc/r3.4.13/zookeeperAdmin.html#sc_zkMulitServerSetup) - Official website
- [Download and Install Apache Zookeeper on Ubuntu](http://www.techburps.com/misc/download-and-install-apache-zookeepr/36)
