---
title: "hadoop名词解释"
tags: ["QJM", "shuffle"]
date: 2020-04-14T03:08:08+08:00
---

QJM and shuffle
==============

# 1. QJM
## 1.1 背景
自从hadoop2版本开始，社区引入了NameNode高可用方案。NameNode主从节点间需要同步操作日志来达到主从节点元数据一致。最初业界均通过NFS来实现日志同步，大家之所以选择NFS，一方面因为可以很方便地实现数据共享，另外一方面因为NFS已经发展20多年，已经相对稳定成熟。

虽然如此，NFS也有缺点不能满足HDFS的在线存储业务：网络单点及其存储节点单点。业界提供了数据共享的一些高可用解决方案，但均不能很好地满足目前HDFS的应用场景。

|方案	|网络单点	|存储单点	|备注|
|-|-|-|-|
|Mysql |HA	|无	|无	|数据有丢失风险
|Drbd+heartbeat+NFS	|无	|无	|脑裂；数据有丢失风险
|Keepalive+NFS	|无	|有	|数据有丢失风险

为了满足共享日志的高可用性，社区引入QJM。QJM由cloudera开发，实现了读写高可用性，使HDFS达到真正的高可用性成为可能。

## 1.2 术语和定义

|术语和定义	|解释|
|-|-|
|Epoch	|由主节点在启动及其切换为主的时候分配，每次操作JN节点均会检查该值,类似zookeeper中的zxid，此时主NameNode类似zookeeper中的leader，JN节点类似ZK中的Follower|
|JournalNode	|QJM存储段进程，提供日志读写，存储，修复等服务|
|QJM	|Qurom Journal Manager|
|startLogSegment	|开始一个新的日志段,该日志段状态为接收写入日志的状态|
|finalizeLogSegment	|将文件由正在写入日志的状态转化为不接收写日志的状态|
|recoverUnfinalizedSegments	|主从切换等情况下，恢复没有转换为finalized状态的日志|
|journalId	|日志ID，由配置指定，如qjournal://g42:8485;g35:8485;uhp9:8485/geminifs，则其中的geminifs即为journalId|

# 2. shuffle

作者：Lijie Xu
链接：https://www.zhihu.com/question/27643595/answer/127473409
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


1. 从逻辑角度来讲，Shuffle 过程就是一个 GroupByKey 的过程，两者没有本质区别。
只是 MapReduce 为了方便 GroupBy 存在于不同 partition 中的 key/value records，就提前对 key 进行排序。Spark 认为很多应用不需要对 key 排序，就默认没有在 GroupBy 的过程中对 key 排序。

2. 从数据流角度讲，两者有差别。
MapReduce 只能从一个 Map Stage shuffle 数据，Spark 可以从多个 Map Stages shuffle 数据（这是 DAG 型数据流的优势，可以表达复杂的数据流操作，参见 CoGroup(), join() 等操作的数据流图 SparkInternals/4-shuffleDetails.md at master · JerryLead/SparkInternals · GitHub）。

3. Shuffle write/read 实现上有一些区别。
以前对 shuffle write/read 的分类是 sort-based 和 hash-based。MapReduce 可以说是 sort-based，shuffle write 和 shuffle read 过程都是基于key sorting 的 (buffering records + in-memory sort + on-disk external sorting)。早期的 Spark 是 hash-based，shuffle write 和 shuffle read 都使用 HashMap-like 的数据结构进行 aggregate (without key sorting)。但目前的 Spark 是两者的结合体，shuffle write 可以是 sort-based (only sort partition id, without key sorting)，shuffle read 阶段可以是 hash-based。因此，目前 sort-based 和 hash-based 已经“你中有我，我中有你”，界限已经不那么清晰。

4. 从数据 fetch 与数据计算的重叠粒度来讲，两者有细微区别。
MapReduce 是粗粒度，reducer fetch 到的 records 先被放到 shuffle buffer 中休息，当 shuffle buffer 快满时，才对它们进行 combine()。而 Spark 是细粒度，可以即时将 fetch 到的 record 与 HashMap 中相同 key 的 record 进行 aggregate。

5. 从性能优化角度来讲，Spark考虑的更全面。
MapReduce 的 shuffle 方式单一。Spark 针对不同类型的操作、不同类型的参数，会使用不同的 shuffle write 方式。比如 Shuffle write 有三种实现方式：

![2020-04-15 03:58:47](https://gitee.com/xiaobin80/csdn/raw/master/images/202004150355001.png)

其中 Serialized sorting 方式既可以使用堆内内存，也可以使用堆外内存。更多的细节就不详述了，感兴趣可以看相关的实现类。

# Reference
- [HDFS之Qurom Journal Manager（QJM）实现机制分析](https://blog.csdn.net/lifuxiangcaohui/article/details/25338985)
- [park的shuffle和Hadoop的shuffle（mapreduce)的区别和关系是什么？](https://www.zhihu.com/question/27643595)
​
