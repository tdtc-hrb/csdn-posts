---
title: "架设Real Server和Keepalived Server"
description: "本系列包括nginx负载平衡、Keepalive安装配置"
date: 2022-10-03T02:38:08+08:00
---
准备工作：
- [修改网卡为固定IP](https://tdtc-hrb.github.io/csdn/post/ops_network_adapter_linux/)
- [开启sudo](https://tdtc-hrb.github.io/csdn/post/ops_sudo/)


# 总览
OS:  minimal
- [Alma](https://mirrors.almalinux.org/isos/x86_64/8.6.html)
- [CentOS7.9](http://isoredirect.centos.org/centos/7/isos/x86_64/)

## Real Server
- RHEL78RS1
> 192.168.0.127
1. Nginx v1.22
2. Tomcat v8.5.82

- RHEL78RS2
> 192.168.0.122
1. Nginx v1.22
2. Tomcat v9.0.68

## Keepalived Server
- RHEL78Master
> 192.168.0.129

- RHEL78Bak
> 192.168.0.130

# 一、Real Server
安装前准备:
- [Linux下安装Tomcat](https://tdtc-hrb.github.io/csdn/post/ops_tomcat/)
- [nginx - CentOS7](https://tdtc-hrb.github.io/csdn/post/ops_nginx/)
- [nginx - RHEL8](https://tdtc-hrb.github.io/cnblogs/post/ops_nginx-v2/)

## config ARP
ifconfig:
```bash
$sudo yum install net-tools
```

### Service
```bash
#vi /etc/init.d/realserver
```
/etc/init.d/realserver:
```bash
#!/bin/bash

SNS_VIP=192.168.0.220

case "$1" in
start)
       ifconfig lo:0 $SNS_VIP netmask 255.255.255.255 broadcast $SNS_VIP
       /sbin/route add -host $SNS_VIP dev lo:0
       echo "1" >/proc/sys/net/ipv4/conf/lo/arp_ignore
       echo "2" >/proc/sys/net/ipv4/conf/lo/arp_announce
       echo "1" >/proc/sys/net/ipv4/conf/all/arp_ignore
       echo "2" >/proc/sys/net/ipv4/conf/all/arp_announce
       sysctl -p >/dev/null 2>&1
       echo "RealServer Start OK"
       ;;
stop)
       ifconfig lo:0 down
       route del $SNS_VIP >/dev/null 2>&1
       echo "0" >/proc/sys/net/ipv4/conf/lo/arp_ignore
       echo "0" >/proc/sys/net/ipv4/conf/lo/arp_announce
       echo "0" >/proc/sys/net/ipv4/conf/all/arp_ignore
       echo "0" >/proc/sys/net/ipv4/conf/all/arp_announce
       echo "RealServer Stoped"
       ;;
*)
       echo "Usage: $0 {start|stop}"
       exit 1
esac
exit 0
```

## exec
```bash
$sudo chmod 755 /etc/init.d/realserver
$sudo service realserver start
```

- tomcat
```bash
/usr/local/tomcat/bin/startup.sh
```

- nginx
```bash
sudo /usr/sbin/nginx
```


# 二、Keepalived Server
- [v1](https://tdtc-hrb.github.io/csdn/post/ops_keepalived/)
- [v2](https://tdtc-hrb.github.io/cnblogs/post/ops_keepalived_v2/)
```bash
#systemctl start keepalived
```

## see state
virtual IP show:
```bash
#ip addr show
```
see IP Virtual Server:
```bash
#ipvsadm -Ln
```bash
see log:
```bash
#journalctl -u keepalived
```

## Version differences
v1 VS v2
### nb_get_retry - v2
[nb_get_retry](https://github.com/acassen/keepalived/issues/1954#issuecomment-892820699) is a deprecated keyword that was only used for SSL_GET and HTTP_GET checkers.    
The keyword that should be used, and it applies to all relevant checkers, is retry.


# 三、运行效果
打开网址时:
![tomcat 8.5.82](https://github.com/tdtc-hrb/csdn/raw/master/images/tomcat85x.png)

刷新后:
![tomcat 9.0.67](https://github.com/tdtc-hrb/csdn/raw/master/images/tomcat90x.png)
