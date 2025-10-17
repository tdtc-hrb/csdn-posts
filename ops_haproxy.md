---
title: "安装配置haproxy"
description: "OS: CentOS7"
date: 2020-05-04T18:08:08+08:00
---
# 系统设置

## 新增用户
```bash
$useradd haproxy
$passwd haproxy
```

## sysctl
> /etc/sysctl.conf

```bash
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 1024 65023
net.ipv4.tcp_max_syn_backlog = 10240
net.ipv4.tcp_max_tw_buckets = 400000
net.ipv4.tcp_max_orphans = 60000
net.ipv4.tcp_synack_retries = 3
net.core.somaxconn = 10000
```

## firewall
### 1) L7
```bash
$firewall-cmd --permanent --zone=public --add-service=http

$firewall-cmd --permanent --zone=public --add-port=6080/tcp
```

### 2) L4
> mysql

```bash
$firewall-cmd --permanent --zone=public --add-port=13306/tcp
```
生效
```bash
$firewall-cmd --reload
```


# rsyslog的安装
## install
```bash
$yum install rsyslog
```

## config
### 1) rsyslog
> /etc/rsyslog.d/haproxy.conf

```bash
$ModLoad imudp
$UDPServerRun 514
$template Haproxy,”%msg%\n”
local2.=info -/var/log/haproxy.log;Haproxy
local2.notice -/var/log/haproxy-status.log;Haproxy
### keep logs in localhost ##
local2.* ~
```

### 2) rotated log
> /etc/logrotate.d/haproxy

```bash
/var/log/haproxy.log {
missingok
notifempty
sharedscripts
rotate 120
daily
compress
postrotate
reload rsyslog >/dev/null 2>&1 || true
endscript
}
```

## exec
```bash
$systemctl restart rsyslog.service
```


# 安装haproxy
## install
[haproxy-1.8.13.tar.gz](https://pan.baidu.com/s/1KVqOvHMl65m0BO3wOcUabw)

### 1) depend
```bash
$yum install pcre* openssl-devel
```

### 2) make install
```bash
$tar xzvf haproxy-1.8.13.tar.gz
$cd haproxy-1.8.13
$make TARGET=linux2628 USE_STATIC_PCRE=1
```

> about TARGET:
>>
```
https://github.com/haproxy/haproxy/blob/master/README
TARGET variable :
  - linux2628   for Linux 2.6.28, 3.x, and above (enables splice and tproxy)
  - generic     for any other OS or version.
```


Install the HAproxy as user root,
```bash
$make install
```

### 3) setup
#### oper fs
```bash
$mkdir -p /etc/haproxy

$mkdir -p /var/lib/haproxy

$touch /var/lib/haproxy/stats
```
```bash
$ln -s /usr/local/sbin/haproxy /usr/sbin/haproxy
```

#### Run automatically
```bash
$cp /home/haproxy/haproxy-1.8.13/examples/haproxy.init /etc/init.d/haproxy

$chmod 755 /etc/init.d/haproxy

$systemctl daemon-reload

$chkconfig haproxy on
```

## config file
> /etc/haproxy/haproxy.cfg

```bash
global
    log         127.0.0.1 local2
    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    stats socket /var/lib/haproxy/stats

defaults
    mode                    tcp
    log                     global
    retries                 3
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout check           5s
    maxconn                 3000

listen mysql
    bind 0.0.0.0:13306
    mode tcp
    balance leastconn

    server mysql1 192.168.0.110:3306 check inter 3000
    server mysql2 192.168.0.111:3306 check inter 3000

listen stats
    bind 0.0.0.0:6080
    mode            http
    log             global

    maxconn 10

    timeout client  100s
    timeout server  100s
    timeout connect 100s
    timeout queue   100s

    stats enable
    stats hide-version
    stats refresh 30s
    stats uri /stats
    stats show-node
```

## test
http://192.168.0.105:6080/stats


# Reference
- [How to enable logging of haproxy in rsyslog](http://sharadchhetri.com/2013/10/16/how-to-enable-logging-of-haproxy-in-rsyslog/)
- [How to Install HAProxy Load Balancer on CentOS](https://www.upcloud.com/support/haproxy-load-balancer-centos/)
