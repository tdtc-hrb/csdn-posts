---
title: "Tomcat安装"
description: "Linux下tomcat的配置"
date: 2025-11-03T12:38:08+08:00
---

|Servlet Spec|JSP Spec|EL Spec|WebSocket Spec|Authentication (JASIC) Spec|Apache Tomcat Version|Latest Released Version|Supported Java Versions|
|-|-|-|-|-|-|-|-|
|6.1|4.0|6.0|2.2|3.1|11.0.x|11.0.0|17 and later|
|3.1|2.3|3.0|1.1|1.1|8.5.x|[8.5.100](https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.100/bin/apache-tomcat-8.5.100.zip)|7 and later|
|2.5|2.1|2.1|N/A|N/A|6.0.x|[6.0.53](https://archive.apache.org/dist/tomcat/tomcat-6/v6.0.53/bin/apache-tomcat-6.0.53.zip)|5 and later|
|2.4|2.0|N/A|N/A|N/A|5.5.x|[5.5.36](https://archive.apache.org/dist/tomcat/tomcat-5/v5.5.36/bin/apache-tomcat-5.5.36.zip)|1.4 and later|

# installation
|Platform version|Released|Specification|Java SE Support|Important Changes|
|-|-|-|-|-|
|Jakarta EE 10|2022-03-31|10|Java SE 17/Java SE 11|Removal of deprecated items in Servlet, Faces, CDI and EJB (Entity Beans and Embeddable Container). CDI-Build Time.|
|Jakarta EE 9|2020-12-08|9|Java SE 8|API namespace move from javax to jakarta|
|Jakarta EE 8|2019-09-10|8|Java SE 8|Full compatiblity with Java EE 8|
|Java EE 8|2017-08-31|JSR 366|Java SE 8|HTTP/2 and CDI based Security|

## 安装JDK
- RHEL Community    
  参见 [设置OpenJDK - RHEL](https://tdtc-hrb.github.io/cnblogs/post/ops_openjdk_rhel/)
- Ubuntu    
  参见 [设置OpenJDK - Ubuntu](https://tdtc-hrb.github.io/csdn/post/ops_openjdk_ubuntu/)


## 安装Tomcat
```bash
$sudo dnf install wget tar
```
下载(choose one):
- v9.0
```
$wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.111/bin/apache-tomcat-9.0.111.tar.gz --no-check-certificate
```
- v11.0
```bash
$wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-11/v11.0.13/bin/apache-tomcat-11.0.13.tar.gz --no-check-certificate
```

Example of extracting v9.0:
```bash
$tar -zxvf apache-tomcat-9.0.111.tar.gz
```

### 移动文件夹
拷贝并重命名为tomcat
```bash
$sudo cp -R apache-tomcat-9.0.111 /usr/local/tomcat
```


# 执行Tomcat
allow fw:
- rhel
```bash
$sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
$sudo firewall-cmd --reload
```

- centos5/6
```bash
$sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
```

## 授权
给tdtc(普通用户)有执行Tomcat的权利:
```bash
$su - root
```
授权文件夹:
```bash
#chown -R tdtc /usr/local/tomcat
```
返回普通用户:
```bash
#exit
```

## 管理app
增加manager-gui角色，并授权给tomcat用户。
```bash
$vi /usr/local/tomcat/conf/tomcat-users.xml
```
tomcat-users.xml:
```bash
  <role rolename="tomcat"/>
  <role rolename="manager-gui"/>
  <user username="tomcat" password="tomcat" roles="tomcat,manager-gui"/>
```

启动tomcat:
```bash
$/usr/local/tomcat/bin/startup.sh
```
