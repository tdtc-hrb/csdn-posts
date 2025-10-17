---
title: "mysql的1067错误"
description: "mysql v5.0/5.1"
date: 2020-05-15T06:08:08+08:00
---

# 日志文件问题
由于使用时间比较长积累了很多的日志文件（\data目录下），所以删除之！
```bash
mysql-bin.000001
... ...
mysql-bin.000278
```

原来删除都留最后一个编号的日志，这次太过彻底~~都删了!

执行net start mysql：
![cmd info](https://gitee.com/xiaobin80/csdn/raw/master/images/2020051514104614.png)
出现了mysql 1067错误！    
查看，主机名.err文件（lenovo-7c8f808d.err）：
```bash
130416  9:59:23  InnoDB: Started; log sequence number 0 43655
MySQL: File '.\mysql-bin.000278' not found (Errcode: 2)
130416  9:59:23 [ERROR] Failed to open log (file '.\mysql-bin.000278', errno 2)
130416  9:59:23 [ERROR] Could not open log file
130416  9:59:23 [ERROR] Can't init tc log
130416  9:59:23 [ERROR] Aborting

130416  9:59:23  InnoDB: Starting shutdown...
130416  9:59:24  InnoDB: Shutdown completed; log sequence number 0 43655
130416  9:59:24 [Note] MySQL: Shutdown complete
```

告诉我们找不到mysql-bin.000278文件！    
既然找不到，我们新弄一个日志文件：    
打开my.ini文件
![cmd info](https://gitee.com/xiaobin80/csdn/raw/master/images/2020051514154614.png)

重新启动mysql服务：
![cmd info](https://gitee.com/xiaobin80/csdn/raw/master/images/2020051514164614.png)
正常启动mysql了！

# 系统表问题
由于要升级MySQL到V5.6，所以拷贝my.ini和数据文件到新的系统上。    
在启动服务时，又出现1067错误！    
查看，主机名.err文件（xiaobin-PC.err）
```bash
2013-12-02 20:23:22 3684 [Note] Plugin 'FEDERATED' is disabled.
2013-12-02 20:23:22 171c InnoDB: Warning: Using innodb_additional_mem_pool_size is DEPRECATED. This option may be removed in future releases, together with the option innodb_use_sys_malloc and with the InnoDB's internal memory allocator.
2013-12-02 20:23:22 3684 [Note] InnoDB: The InnoDB memory heap is disabled
2013-12-02 20:23:22 3684 [Note] InnoDB: Mutexes and rw_locks use Windows interlocked functions
2013-12-02 20:23:22 3684 [Note] InnoDB: Compressed tables use zlib 1.2.3
2013-12-02 20:23:22 3684 [Note] InnoDB: Not using CPU crc32 instructions
2013-12-02 20:23:22 3684 [Note] InnoDB: Initializing buffer pool, size = 47.0M
2013-12-02 20:23:22 3684 [Note] InnoDB: Completed initialization of buffer pool
2013-12-02 20:23:22 3684 [Note] InnoDB: Highest supported file format is Barracuda.
2013-12-02 20:23:23 3684 [Warning] InnoDB: Resizing redo log from 2*3072 to 2*1536 pages, LSN=1625977
2013-12-02 20:23:23 3684 [Warning] InnoDB: Starting to delete and rewrite log files.
2013-12-02 20:23:23 3684 [Note] InnoDB: Setting log file .\ib_logfile101 size to 24 MB
2013-12-02 20:23:23 3684 [Note] InnoDB: Setting log file .\ib_logfile1 size to 24 MB
2013-12-02 20:23:24 3684 [Note] InnoDB: Renaming log file .\ib_logfile101 to .\ib_logfile0
2013-12-02 20:23:24 3684 [Warning] InnoDB: New log files created, LSN=1625977
2013-12-02 20:23:24 3684 [Note] InnoDB: 128 rollback segment(s) are active.
2013-12-02 20:23:24 3684 [Note] InnoDB: Waiting for purge to start
2013-12-02 20:23:24 3684 [Note] InnoDB: 5.6.14 started; log sequence number 1625977
2013-12-02 20:23:24 3684 [ERROR] mysql56: unknown variable 'table_cache=256'
2013-12-02 20:23:24 3684 [ERROR] Aborting
```

从err文件中可以看到错误主要是“未知变量‘table_cache=256’”。

  在"[系统变量](https://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html)"中我们找到三个以“table”开头的全局变量：    
- table_definition_cache     
系统缺省设置；
- table_open_cache
出错部分的变量
- table_open_cache_instances    
系统缺省实例数；

在mysql 5.1文档里找到了答案：
从v5.1.3版本开始已经移除了table_cache!

|Deprecated|5.1.3, by table_open_cache|    
|-|-:|
|Removed|5.1.3|    

This is the old name of table_open_cache before MySQL 5.1.3. From 5.1.3 on, use table_open_cache instead.

- 解决办法：
把ini中的table_cache替换为table_open_cache。
