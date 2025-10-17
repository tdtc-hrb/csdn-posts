---
title: "Hadoop配置 - base"
description: "Hadoop cluster共4篇，其余Hadoop配置 - HDFS、Hadoop配置 - YARN、Hadoop - cluster"
date: 2020-04-14T08:08:08+08:00
---
> (../etc/hadoop)

# 1. core-site.xml

## 1) HDFS
> the default path prefix used by the Hadoop FS client when none is given
Optionally, you may now configure the default path for Hadoop clients to use the new HA-enabled logical URI.
If you used “mycluster” as the nameservice ID earlier, this will be the value of the authority portion of all of your HDFS paths.
This may be configured like so, in your core-site.xml file:

```xml
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://mycluster</value>
</property>
```

## 2) Zookeeper

### addr
```xml
<property>
    <name>ha.zookeeper.quorum</name>
    <value>tdtc201:2181,tdtc202:2181,tdtc203:2181,tdtc204:2181</value>
</property>
```

### timeout
> [Verifying automatic failover](https://hadoop.apache.org/docs/r2.7.7/hadoop-project-dist/hadoop-hdfs/HDFSHighAvailabilityWithNFS.html)

>> The amount of time required to detect a failure and trigger a fail-over depends on the configuration of ha.zookeeper.
session-timeout.ms, but defaults to 5 seconds.

设置小于5秒的任意值即可（4999~1ms）
```xml
<property>
    <name>ha.zookeeper.session-timeout.ms</name>
    <value>3000</value>
    <description>ms</description>
</property>
```

# 2. mapred-site.xml

## 1) framework
```xml
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
```

> 根据官方doc也可使用“local”

## 2) jobhistory

### IPC
MapReduce JobHistory Server IPC host:port
```xml
<property>
    <name>mapreduce.jobhistory.address</name>
    <value>tdtc201:10020</value>
</property>
```


### Web UI
MapReduce JobHistory Server Web UI host:port
```xml
<property>
    <name>mapreduce.jobhistory.webapp.address</name>
    <value>tdtc201:19888</value>
</property>
```

# Reference
- [HDFS High Availability](https://hadoop.apache.org/docs/r2.7.7/hadoop-project-dist/hadoop-hdfs/HDFSHighAvailabilityWithNFS.html) - fs.defaultFS
- [core-default.xml](https://hadoop.apache.org/docs/r2.7.7/hadoop-project-dist/hadoop-common/core-default.xml)
- [mapred-default.xml](https://hadoop.apache.org/docs/r2.7.7/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml)
