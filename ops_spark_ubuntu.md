---
title: "安装spark"
description: "ubuntu"
date: 2024-03-15T08:08:08+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

> WSL: Ubuntu 18.04.1

# Prepare
> Java 8+, Python 2.7+/3.4+ and R 3.1+
```bash
Spark runs on Java 8+, Python 2.7+/3.4+ and R 3.1+.
For the Scala API, Spark 2.4.1 uses Scala 2.12.
You will need to use a compatible Scala version (2.12.x).
```

## 1. java
[设置OpenJDK - Ubuntu](https://xiaobin80.gitee.io/csdn/post/ops_openjdk_ubuntu/)

## 2. Python
```bash
sudo apt-get install python
```

## 3. R language
### 1) add ppa
```bash
sudo add-apt-repository ppa:marutter/rrutter
sudo apt-get update
```
### 2) install
```bash
sudo apt-get install r-base r-base-dev
```

# Installation
## 1. unzip
```bash
~/spark-2.4.1-bin-hadoop2.7
```

## 2. test
```bash
cd ~/spark-2.4.1-bin-hadoop2.7
```

### 1) pi
```bash
./bin/run-example SparkPi 10
```

### 2) Scala shell
```bash
./bin/spark-shell --master local[2]
```

input:
```bash
for (i <- 1 to 3; j <- 1 to 3) print(10 * i + j + "\t")
```

quit:
```bash
:q
```

### 3) pyspark
```bash
./bin/spark-submit examples/src/main/python/pi.py 10
```

### 4) sparkR
```bash
./bin/sparkR --master local[2]
```

input:
```bash
print(matrix(c(.3,  .6,  .9, .3 + .6)), digits = 18)
```

quit:
```bash
q()
```

### 5) R example
```bash
./bin/spark-submit examples/src/main/r/dataframe.R
```

# run
```bash
cd ~/spark-2.4.1-bin-hadoop2.7
./sbin/start-master.sh
```

web: http://localhost:8080/
![spark info](https://gitee.com/xiaobin80/csdn/raw/master/images/20190420164245413.png)


# Reference
- [Installing Scala and Spark on Ubuntu](https://medium.com/@josemarcialportilla/installing-scala-and-spark-on-ubuntu-5665ee4b62b1)
- [installing-r-in-ubuntu](http://sites.psu.edu/theubunturblog/installing-r-in-ubuntu/)
