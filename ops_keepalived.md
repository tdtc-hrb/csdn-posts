---
title: "安装配置Keepalived"
description: "本系列包括架设Real Server和Keepalived Server、nginx负载平衡"
date: 2022-04-13T08:38:08+08:00
---
> OS: CentOS7 minimal(VMware)

# 一、安装

## 0. prepare tool
```bash
$yum group list
```

### 1) dev tools
```bash
$yum group install "Development Tools"
```

### 2) openssl-devel
> update see "nginx’s load balancing的安装部分的[update OpenSSL](https://xiaobin80.gitee.io/csdn/post/ops_nginx/)"
```bash
$yum install openssl-devel
```

### 3) libnl
```bash
$yum install libnl libnl-devel libnfnetlink-devel
```

## 1. keepalived
> v1.4.5

### down and unzip
```bash
$wget http://www.keepalived.org/software/keepalived-1.4.5.tar.gz
$ tar zxvf keepalived-1.4.5.tar.gz
```

### installation
```bash
$cd keepalived-1.4.5
$./configure --prefix=/usr/local/keepalived
$make
```

Root permission Required
```bash
$make install
```

### setup
```bash
$cp /usr/local/keepalived/sbin/keepalived /usr/sbin/
$mkdir -p /etc/keepalived
$cp /usr/local/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf
$cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
$cp /tmp/keepalived-1.4.5/keepalived/etc/init.d/keepalived /etc/init.d/
$chmod 755 /etc/init.d/keepalived
```

edit service config file
```bash
$vi /lib/systemd/system/keepalived.service
```

> vi command mode:
```bash
%s/\/usr\/local\/keepalived//g
```

- test
```bash
$keepalived -v
```

## 2. close selinux and fw

### 1) selinux
```bash
$vi /etc/selinux/config
```

更改enforcing为disabled
>> enforcing -> disabled

### 2) fw
```bash
$systemctl stop firewalld.service
$systemctl disable firewalld.service
```

# 二、配置
> (/etc/keepalived/keepalived.conf)

## master
### 1) global_defs
>  router_id
```bash
router_id LVS_MASTER
```

### 2) VI-1
#### interface
```bash
interface ens32
```

#### priority
> default
```bash
priority 100
```

### 3) v-ip
```bash
virtual_ipaddress {
    192.168.0.220
}
```

### 4) lb algo
> 轮叫（Round Robin）、     
加权轮叫（Weighted Round Robin）、    
最少链接（Least Connections）、     
加权最少链接（Weighted Least Connections）、    
基于局部性的最少链接（Locality-Based Least Connections）、    
带复制的基于局部性最少链接（Locality-Based Least Connections with Replication）、    
目标地址散列（Destination Hashing）、    
源地址散列（Source Hashing）

```bash
lb_algo wrr
```

### 5) lb kind
```bash
lb_kind DR
```

### 6) realserver
#### weight

```bash
weight 3
```

#### tcp check
```bash
TCP_CHECK {  
connect_timeout 10         
nb_get_retry 3  
delay_before_retry 3  
connect_port 80  
}
```

## Slave
> Different from master::

### 1) global_defs
> router_id
```bash
router_id LVS_SLAVE
```

### 2) VI-1

#### state
```bash
state BACKUP
```

#### priority
```bash
priority 70
```

# example
> keepalived.conf(Master)

```bash
! Configuration File for keepalived

global_defs {
   ##notification_email {
   ##  acassen@firewall.loc
   ##  failover@firewall.loc
   ##  sysadmin@firewall.loc
   ##}
   ##notification_email_from Alexandre.Cassen@firewall.loc
   ##smtp_server 192.168.200.1
   ##smtp_connect_timeout 30
   router_id LVS_MASTER
   vrrp_skip_check_adv_addr
   ##vrrp_strict
   vrrp_garp_interval 0
   vrrp_gna_interval 0
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        ##192.168.200.16
        ##192.168.200.17
        ##192.168.200.18
        192.168.0.220
    }
}

##virtual_server 192.168.200.100 443 {
##    delay_loop 6
##    lb_algo rr
##    lb_kind NAT
##    persistence_timeout 50
##    protocol TCP

##    real_server 192.168.201.100 443 {
##        weight 1
##        SSL_GET {
##            url {
##              path /
##              digest ff20ad2481f97b1754ef3e12ecd3a9cc
##            }
##            url {
##              path /mrtg/
##              digest 9b3a0c85a887a256d6939da88aabd8cd
##            }
##            connect_timeout 3
##            retry 3
##            delay_before_retry 3
##        }
##    }
##}

virtual_server 192.168.0.220 80 {
    delay_loop 6
    lb_algo wrr ## rr
    lb_kind DR  ## NAT
    persistence_timeout 50
    protocol TCP

    ##sorry_server 192.168.200.200 1358

    real_server 192.168.0.107 80 {
        weight 3
        TCP_CHECK {  
            connect_timeout 10         
            nb_get_retry 3  
            delay_before_retry 3  
            connect_port 80  
        }
    }

    real_server 192.168.0.102 80 {
        weight 1
        TCP_CHECK {  
            connect_timeout 10         
            nb_get_retry 3  
            delay_before_retry 3  
            connect_port 80  
        }
    }
}
```

# Reference
- [official doc](http://www.keepalived.org/doc/installing_keepalived.html) - installing keepalived
- [CentOS / RHEL 7: Install GCC (C and C++ Compiler) and Development Tools](https://www.cyberciti.biz/faq/centos-rhel-7-redhat-linux-install-gcc-compiler-development-tools/)
- [Centos7.4下keepalived-1.3.5的安装使用](https://www.cnblogs.com/flying607/p/8882617.html)
