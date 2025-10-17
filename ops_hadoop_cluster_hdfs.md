---
title: "Hadoop配置 - hdfs"
description: "Hadoop cluster共4篇，其余Hadoop配置 - base、Hadoop配置 - YARN、Hadoop - cluster"
date: 2020-04-14T09:08:08+08:00
---
> (../etc/hadoop/hdfs-site.xml)

# 1. name services
```xml
<property>
  <name>dfs.nameservices</name>
  <value>mycluster</value>
</property>
```

## 1) name node
```xml
<property>
  <name>dfs.ha.namenodes.mycluster</name>
  <value>nn1,nn2</value>
</property>
```

## 2) RPC(name node)
```xml
<property>
  <name>dfs.namenode.rpc-address.mycluster.nn1</name>
  <value>tdtc201:8020</value>
</property>
<property>
  <name>dfs.namenode.rpc-address.mycluster.nn2</name>
  <value>tdtc202:8020</value>
</property>
```

## 3) Http(name node)
```xml
<property>
  <name>dfs.namenode.http-address.mycluster.nn1</name>
  <value>tdtc201:50070</value>
</property>
<property>
  <name>dfs.namenode.http-address.mycluster.nn2</name>
  <value>tdtc202:50070</value>
</property>
```

# 2. JournalNode
## 1) quorum journal manager

```xml
<!-- Journal Node(QJM) -->
<property>
  <name>dfs.namenode.shared.edits.dir</name>
  <value>qjournal://tdtc201:8485;tdtc202:8485;tdtc203:8485/mycluster</value>
</property>
```

### local
```xml
<property>
  <name>dfs.journalnode.edits.dir</name>
  <value>/home/tdtc/app/hadoop/data/journaldata</value>
</property>
```


# 3. failover
## 1) support class
```xml
<!-- the Java class that HDFS clients use to contact the Active NameNode -->
<property>
  <name>dfs.client.failover.proxy.provider.mycluster</name>
  <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
</property>
```

## auto switch
```xml
<property>
  <name>dfs.ha.automatic-failover.enabled</name>
  <value>true</value>
</property>
```

# 4. sshfence
```xml
<!-- sshfence - SSH to the Active NameNode and kill the process -->
<property>
  <name>dfs.ha.fencing.methods</name>
  <value>sshfence</value>
</property>
<property>
  <name>dfs.ha.fencing.ssh.private-key-files</name>
  <value>/home/tdtc/.ssh/id_rsa</value>
</property>
```

## 1) timeout
> default: 30s

```xml
<property>
  <name>dfs.ha.fencing.ssh.connect-timeout</name>
  <value>30000</value>
</property>
```

# Reference
- [HDFS High Availability With QJM](https://hadoop.apache.org/docs/r2.7.7/hadoop-project-dist/hadoop-hdfs/HDFSHighAvailabilityWithQJM.html)
- [hdfs-default.xml](https://hadoop.apache.org/docs/r2.7.7/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml)
- [Configuring Hadoop High Availability Clusters](https://www.i-programmer.info/projects/31-systems/11781-configuring-hadoop-high-availability-clusters.html)
