---
title: "Hadoop - cluster"
description: "Hadoop cluster共4篇，其余Hadoop配置 - base、Hadoop配置 - hdfs、Hadoop - yarn"
date: 2022-10-11T00:08:08+08:00
---

# Prepare
| |server1|server2|server3|server4|
|-|-|-|-|-|
|NameNode	|y	|y | | |
|DataNode	| 	|y	|y	|y|
|ResourceManager	| 	| 	|y	|y|
|NodeManager	|y	|y	|y	|y|
|Zookeeper	|y	|y	|y	|y|
|JournalNode	|y	|y	|y| |
|ZK-FC	|y	|y	| | |
|JobHistor |y	 |  |	| | 


|Server Name	|IP	|Host Name|
|-|-|-|
|server1	|192.168.42.107|tdtc201|
|server2	|192.168.42.108|tdtc202|
|server3	|192.168.42.109|tdtc203|
|server4	|192.168.42.110|tdtc204|

## 1. zk
```bash
zkServer.sh start
```
安装详见《[ZooKeeper multi-server install](https://tdtc-hrb.github.io/csdn/post/ops_zookeeper/)》

## 2. Open [firewall](https://tdtc-hrb.github.io/csdn/post/ops_firewall/)


# Stage I: Installation

## 1. Unzip
```bash
/home/$USER/app/hadoop/$version
```

> 路径参考[Oracle11g](https://www.tecmint.com/oracle-database-11g-release-2-installation-in-linux/)

```bash
mkdir -p ~/app/hadoop
```
```bash
tar zxvf hadoop-2.7.7.tar.gz -C ~/app/hadoop/
```
```bash
mv ~/app/hadoop/hadoop-2.7.7 ~/app/hadoop/2.7.7
```

## 2. set up
> 详见Reference[1][2][3]。

### 1) 再显式地重新声明一遍JAVA_HOME
> [hadoop-env.sh](https://www.cnblogs.com/codeOfLife/p/5940642.html)

```bash
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.345.b01-1.el7_9.x86_64
```

### 2) set slaves
这个设置文件决定有几个DN(data node)!!!

> slaves
```bash
tdtc202
tdtc203
tdtc204
```

## 3. set Hadoop Home
```bash
vi ~/.bashrc
```

```bash
export HADOOP_HOME=/home/tdtc/app/hadoop/2.7.7
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

- test
```bash
[tdtc@tdtc201 ~]$ hadoop version
Hadoop 2.7.7
Subversion Unknown -r c1aad84bd27cd79c3d1a7dd58202a8c3ee1ed3ac
Compiled by stevel on 2018-07-18T22:47Z
Compiled with protoc 2.5.0
From source with checksum 792e15d20b12c74bd6f19a1fb886490
This command was run using /home/tdtc/app/hadoop/2.7.7/share/hadoop/common/hadoop-common-2.7.7.jar
```


# Stage II: Init
- 查看信息    
[jps介绍](https://blog.csdn.net/fwch1982/article/details/7947451)

```bash
#jps
```

## 1. run journalnode
> （server1,2,3）
```bash
hadoop-daemon.sh start journalnode
```

## 2. format nn
> (server1 or 2)
```bash
hadoop namenode -format
```

### 1) copy file
```bash
cd ~/app/hadoop
scp -r tmp/ tdtc201:$PWD
```

### 2) format zk-fc
```bash
hdfs zkfc -formatZK
```

# Stage III: Run

## 1. HDFS
> server1 or 2

```bash
start-dfs.sh
```

> stop cmd:
```bash
stop-dfs.sh
```

### 1) check
```bash
hdfs haadmin -getServiceState nn1

hdfs haadmin -getServiceState nn2
```

## 2. YARN
> server3 or 4

```bash
start-yarn.sh
```

### 1) backup site
```bash
yarn-daemon.sh start resourcemanager
```

### 2) check
```bash
yarn rmadmin -getServiceState rm1

yarn rmadmin -getServiceState rm2
```

## 3. Mapreduce
> server1
```bash
mr-jobhistory-daemon.sh start historyserver
```

web:
http://192.168.42.107:50070/dfshealth.html    
or    
http://192.168.42.108:50070/dfshealth.html

# Stage IV: Stop
```bash
$HADOOP_HOME/sbin/stop-all.sh
```

## 1. Mapreduce
```bash
mr-jobhistory-daemon.sh stop historyserver
```

# FAQ

## IP地址变更
Q: 我的IP地址变更了，怎么办？    
A:
1. 改变系统的IP
   1) /etc/hosts
   2) /etc/sysconfig/network-scripts/ifcfg-xxx

2. 删除hadoop目录
   仅限NN（name node）
   $rm -rf ~/app/hadoop

3. 重新安装NN
   本例子为：Server1和Server2

# Reference
- [Hadoop配置 - base](https://tdtc-hrb.github.io/csdn/post/ops_hadoop_cluster_base/)
- [Hadoop配置 - HDFS](https://tdtc-hrb.github.io/csdn/post/ops_hadoop_cluster_hdfs/)
- [Hadoop配置 - YARN](https://tdtc-hrb.github.io/csdn/post/ops_hadoop_cluster_yarn/)
- [ZooKeeper学习之路 （九）利用ZooKeeper搭建Hadoop的HA集群](https://www.cnblogs.com/qingyunzong/p/8634335.html)
