---
title: "soft-list(port)"
description: "ZooKeeper multi-server install"
date: 2020-04-14T02:08:08+08:00
---

# I. Soft List

## 1. zk
```bash
sudo firewall-cmd --permanent --zone=public --add-port=2181/tcp
sudo firewall-cmd --permanent --zone=public --add-port=2888/tcp
sudo firewall-cmd --permanent --zone=public --add-port=3888/tcp
```

## 2. Hadoop
```bash
sudo firewall-cmd --permanent --zone=public --add-port=10020/tcp
sudo firewall-cmd --permanent --zone=public --add-port=19888/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8020/tcp
sudo firewall-cmd --permanent --zone=public --add-port=50070/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8485/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8088/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8033/tcp
```

## 3. Spark
```bash
sudo firewall-cmd --permanent --zone=public --add-port=6066/tcp
sudo firewall-cmd --permanent --zone=public --add-port=7077/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8080-8088/tcp
```

> 立即生效：
```
sudo firewall-cmd --reload
```

# II. Other cmd
- remove port
```bash
firewall-cmd --permanent --zone=public --remove-port=8080-8081/tcp
```

- 查看
```bash
sudo firewall-cmd --list-all
```
