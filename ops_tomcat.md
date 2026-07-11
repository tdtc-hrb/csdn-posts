---
title: "Tomcat安装"
description: "Linux下tomcat的配置"
date: 2026-07-09T12:38:08+08:00
---

|Servlet Spec|Apache Tomcat Version|Supported Java Versions|
|-|-|-|
|6.1|11.0.x|17 and later|
|4.0|9.0.x|8 and later|

See ["which version"](https://tomcat.apache.org/whichversion.html) for details.

Note: End of support for Tomcat 9.0.x has been announced as 31 March 2027.

# installation
- [Java enterprise platform history](https://en.wikipedia.org/wiki/Jakarta_EE)

|Platform version|Released|Specification|Java SE Support|Important Changes|
|-|-|-|-|-|
|Jakarta EE 11|2025-06-26[10]|11|Java SE 21 </br> Java SE 17|Data|
|Jakarta EE 10|2022-03-31|10|Java SE 17 </br> Java SE 11|Removal of deprecated items in Servlet, Faces, CDI and EJB (Entity Beans and Embeddable Container). CDI-Build Time.|
|Jakarta EE 8|2019-09-10|8|Java SE 8|Full compatibility with Java EE 8|
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
$wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.120/bin/apache-tomcat-9.0.120.tar.gz --no-check-certificate
```
- v11.0
```bash
$wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-11/v11.0.24/bin/apache-tomcat-11.0.24.tar.gz --no-check-certificate
```

Example of extracting v9.0:
```bash
$tar -zxvf apache-tomcat-9.0.120.tar.gz
```

### 移动文件夹
拷贝并重命名为tomcat
```bash
$sudo cp -R apache-tomcat-9.0.120 /usr/local/tomcat
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
