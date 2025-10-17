---
title: "spark配置"
description: "Spark cluster共3篇，其余Spark安装、Spark - cluster"
date: 2020-04-15T01:08:08+08:00
---

# 1. spark env
> spark-env.sh

## 1) hadoop path
```bash
export HADOOP_HOME=/home/tdtc/app/hadoop/2.7.7
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
```

## 2) spark worker
使用的内存和cpu core数
```bash
export SPARK_WORKER_MEMORY=128m
export SPARK_WORKER_CORES=1
```

## 3) recovery(zookeeper)
```bash
export SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=tdtc201:2181,tdtc202:2181,tdtc203:2181,tdtc204:2181 -Dspark.deploy.zookeeper.dir=/spark2"
```

### recoveryMode
```bash
spark.deploy.recoveryMode=ZOOKEEPER
```

### url
```bash
spark.deploy.zookeeper.url=tdtc201:2181,tdtc202:2181,tdtc203:2181,tdtc204:2181
```

### directory
储存在zk中的位置
```bash
spark.deploy.zookeeper.dir=/spark2
```



# 2. slaves
> 注意：有几个YARN，就设置几个slaves！！！
>> ../conf/slaves

```bash
tdtc203
tdtc204
```

# Reference
- [how-to-set-up-spark-with-zookeeper-for-ha](https://stackoverflow.com/questions/24183904/how-to-set-up-spark-with-zookeeper-for-ha)
