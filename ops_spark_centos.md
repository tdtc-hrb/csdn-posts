---
title: "spark安装"
description: "Spark cluster共3篇，其余Spark安装、Spark - cluster"
date: 2020-04-15T00:08:08+08:00
---
> Spark：v2.4

# 一、基础软件
## 1. R-lang
```bash
sudo yum -y install epel-release
sudo yum install R
```
> 注意：如果Java没有安装，会附带着安装OpenJDK。

## 2. Scala
```bash
wget https://downloads.lightbend.com/scala/2.12.8/scala-2.12.8.tgz
tar xvf scala-2.12.8.tgz
sudo mv scala-2.12.8 /usr/lib
sudo ln -s /usr/lib/scala-2.12.8 /usr/lib/scala
```

### 1) add path
```bash
$vi ~/.bashrc
```
```bash
PATH=$PATH:/usr/lib/scala/bin
```

# 二、Spark
## 1. down & unzip
> [v2.4.1](https://archive.apache.org/dist/spark/spark-2.4.1/spark-2.4.1-bin-hadoop2.7.tgz)

```bash
mkdir -p ~/app/spark
tar zxvf spark-2.4.1-bin-hadoop2.7.tgz -C ~/app/spark
mv ~/app/spark/spark-2.4.1-bin-hadoop2.7 ~/app/spark/2.4.1
```

## 2. test
```bash
./bin/spark-shell --master local[2]
```
input:
```bash
for (i <- 1 to 3; j <- 1 to 3 if i != j) print(10 * i + j + "\t")
```

input:(exit)
```bash
:q
```

# 三、开启服务

## 1. 添加系统变量
```bash
echo 'export SPARK_HOME=$HOME/app/spark/2.4.1' >> .bash_profile
echo 'export PATH=$PATH:$SPARK_HOME/bin' >> .bash_profile
```

```bash
$source ~/.bash_profile
```

## 2. open port
```bash
sudo firewall-cmd --permanent --zone=public --add-port=6066/tcp
sudo firewall-cmd --permanent --zone=public --add-port=7077/tcp
sudo firewall-cmd --permanent --zone=public --add-port=8080-8088/tcp
sudo firewall-cmd --reload
```

```bash
sudo firewall-cmd --reload
```

## 3. exec
```bash
$SPARK_HOME/sbin/start-master.sh
```

http://192.168.42.101:8080/    
![2019-04-12 00:20:22](https://gitee.com/xiaobin80/csdn/raw/master/images/20190412002022624.png)

# Reference
- [How To Install Apache Spark on CentOS 7](https://idroot.us/linux/install-apache-spark-centos-7/)
- [spark-standalone](http://spark.apache.org/docs/latest/spark-standalone.html)
