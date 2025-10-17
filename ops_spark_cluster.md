---
title: "spark cluster"
description: "Spark cluster共3篇，其余Spark安装、Spark配置"
date: 2020-04-15T02:08:08+08:00
---
> OS: CentOS7

# Prepare
1. 已经安装了《[ZooKeeper multi-server install](https://xiaobin80.gitee.io/csdn/post/ops_zookeeper/)》
2. 已经安装了《[hadoop - cluster](https://xiaobin80.gitee.io/csdn/post/ops_hadoop_cluster/)》

# Stage I: Installation
1. [spark安装 - CentOS](https://xiaobin80.gitee.io/csdn/post/ops_spark_centos/)
2. Firewall设置请参见《[firewall-soft list(port)](https://xiaobin80.gitee.io/csdn/post/ops_firewall/)》
3. 设置OpenJDK请参见《[设置OpenJDK - CentOS](https://xiaobin80.gitee.io/csdn/post/ops_openjdk_centos/)》
4. set up
> 详见Reference[4]


# Stage II: Run
## 1. ZK
```bash
zkServer.sh start
```

## 2. Hadoop
### 1) HDFS
see 《[hadoop - cluster](https://xiaobin80.gitee.io/csdn/post/ops_hadoop_cluster/) - 3.1 HDFS》

### 2) YARN
see 《[hadoop - cluster](https://xiaobin80.gitee.io/csdn/post/ops_hadoop_cluster/) - 3.2 YARN》


## 3. Spark
>（server3，4）

```bash
$SPARK_HOME/sbin/start-all.sh
```

### 1) other site
```bash
$SPARK_HOME/sbin/start-master.sh
```

Web:
http://192.168.42.109:8082/

http://192.168.42.110:8082/

or
http://192.168.42.109:8081/

http://192.168.42.110:8081/


# Stage III: Stop
```bash
$SPARK_HOME/sbin/stop-all.sh
```

# Reference
1. [Running Spark on YARN](http://spark.apache.org/docs/latest/running-on-yarn.html)
2. [hadoop - cluster](https://xiaobin80.gitee.io/csdn/post/ops_hadoop_cluster/)
3. 《[Spark学习之路 （二）Spark2.3 HA集群的分布式安装](https://www.cnblogs.com/qingyunzong/p/8888080.html)》
4. 《[spark配置](https://xiaobin80.gitee.io/csdn/post/ops_spark_config/)》
