---
title: "Linux修改网卡为固定IP"
description: "需要net-tools/netplan"
date: 2024-03-12T08:38:08+08:00
---
- RHEL
- Diban    
Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

# RHEL-like
- Alma/Rocky
- CentOS    
Less than RHEL version 8

## View
net adapter list:
```
$cd /etc/sysconfig/network-scripts
$ls ifcfg-* -l
```

### ipv4
```bash
$ip addr
```

### ipv6
```bash
$ip -6 addr
```


## Setup
Start job network adapter:
```bash
ONBOOT=yes
```

### static
```bash
$cd /etc/sysconfig/network-scripts
$sudo vi ifcfg-enp0s3
```

```bash
BOOTPROTO=none
IPADDR=192.168.1.186
PREFIX=24
GATEWAY=192.168.1.1
DNS1=211.161.191.235
DNS2=211.161.122.197
```

### IPv6 Network
- IPV6INIT=yes    
This initializes the interface for IPv6 addressing.
- IPV6_AUTOCONF=yes    
This enables the IPv6 auto-configuration for the interface.
- IPV6_DEFROUTE=yes    
This indicates that the default IPv6 route has been assigned to the interface.
- IPV6_FAILURE_FATAL=no    
indicates that the system won’t fail even when IPv6 fails.


### virtual machine
  设置Network Adapter为Bridged

#### net adapter
- vmware    
 default: eth0

- vbox    
 default: enp0s3


# Debian-like
Debian:
```
sudo apt install netplan.io
```

## Ubuntu
```bash
$cd /etc/netplan/
$sudo vi 00-installer-config.yaml
```
- Normal
> 路由器下
```bash
#This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.0.55/24
      gateway4: 192.168.0.1
      nameservers:
              addresses: [192.168.0.1, 192.168.1.1]
  version: 2
```

### nameserver
> 在热点下
- nameserver 1    
设置Host的IP
- nameserver 2    
设置Host的DNS

for example:
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens32:
      #dhcp4: true
      dhcp4: no
      addresses:
        - 192.168.69.119/24
      gateway4: 192.168.69.113
      nameservers:
        addresses: [192.168.69.113, 192.168.69.218]
  version: 2
```
宿主机的IP: 192.168.69.113

宿主机的DNS: 192.168.69.218


# Ref
- [How to Configure IPv6 Network on CentOS/RHEL 8](https://www.tecmint.com/configure-ipv6-network-on-centos-rhel/)
- [How To Manage Ubuntu / Debian Networking using Netplan](https://techviewleo.com/manage-ubuntu-debian-networking-using-netplan/)
