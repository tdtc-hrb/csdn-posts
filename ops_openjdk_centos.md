---
title: "设置OpenJDK"
description: "RHEL/CentOS7"
date: 2022-09-15T08:08:08-08:00
---

# 1. install
> jre
```bash
$sudo yum install java -y
```

## jdk
```bash
$sudo yum install java-1.8.0-openjdk-devel
```


# 2. set path

```bash
$sudo vi /etc/profile
```

```bash
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.345.b01-1.el7_9.x86_64
export CLASSPATH=.:$JAVA_HOME/jre/lib/:.
export PATH=$PATH:$JAVA_HOME/bin
```

```bash
$su - root 
#source /etc/profile
```

## About path
[3.3. INSTALL OPENJDK ON RED HAT ENTERPRISE LINUX](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/installation_guide/install_openjdk_on_red_hat_enterprise_linux) - 2.Optional: Set the JAVA_HOME environment variable

# Reference
- [How to install Java on CentOS 7](https://linuxize.com/post/install-java-on-centos-7/)
- [OpenJDK Version Info](https://wiki.openjdk.java.net/display/jdk8u/Main) - Releases
