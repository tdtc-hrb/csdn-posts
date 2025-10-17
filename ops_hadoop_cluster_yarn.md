---
title: "Hadoop配置 - yarn"
description: "Hadoop cluster共4篇，其余Hadoop配置 - base、Hadoop配置 - hdfs、Hadoop - cluster"
date: 2020-04-14T10:08:08+08:00
---
> (../etc/hadoop/yarn-site.xml)

# 1. ResourceManager HA
```xml
<property>
  <name>yarn.resourcemanager.ha.enabled</name>
  <value>true</value>
</property>
<property>
  <name>yarn.resourcemanager.cluster-id</name>
  <value>cluster1</value>
</property>
<property>
  <name>yarn.resourcemanager.ha.rm-ids</name>
  <value>rm1,rm2</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname.rm1</name>
  <value>tdtc203</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname.rm2</name>
  <value>tdtc204</value>
</property>
<property>
  <name>yarn.resourcemanager.webapp.address.rm1</name>
  <value>tdtc203:8088</value>
</property>
<property>
  <name>yarn.resourcemanager.webapp.address.rm2</name>
  <value>tdtc204:8088</value>
</property>
<property>
  <name>yarn.resourcemanager.zk-address</name>
  <value>tdtc201:2181,tdtc202:2181,tdtc203:2181</value>
</property>
```

# 2. Resource Manager Restart
> see Reference[3]

```xml
<property>
    <name>yarn.resourcemanager.recovery.enabled</name>
    <value>true</value>
</property>
```

## 1) zookeeper
```xml
<property>
    <name>yarn.resourcemanager.store.class</name>
    <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
</property>
```

# 3. Aux Service（Auxiliary Service）

## 1) base
```xml
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
```

## 2) spark
```xml
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>spark_shuffle,mapreduce_shuffle</value>
</property>
```

# 4. log aggregation
1. [yarn-default.xml](https://hadoop.apache.org/docs/r2.7.7/hadoop-yarn/hadoop-yarn-common/yarn-default.xml)
2. [Resource Manager HA](https://hadoop.apache.org/docs/r2.7.7/hadoop-yarn/hadoop-yarn-site/ResourceManagerHA.html)
3. [Resource Manager Restart](https://hadoop.apache.org/docs/r2.7.7/hadoop-yarn/hadoop-yarn-site/ResourceManagerRestart.html)
