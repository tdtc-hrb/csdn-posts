---
title: "设置固定IP"
description: "Ubuntu"
date: 2020-04-16T00:08:08+08:00
---
> 开发板访问NFS 服务器IP地址是固定的。

# 一、U16及以下
> （Ubuntu16.04-）    
使用root权限更改interfaces文件。

```bash
#gedit /etc/network/interfaces
```

完整例子：
> /etc/network/interfaces

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static

address 192.168.0.111
netmask 255.255.255.0
gateway 192.168.0.1
```


# 二、U18及以上
> （Ubuntu18.04+）

```bash
#vi /etc/netplan/50-cloud-init.yaml
```

```bash
network:
    ethernets:
        ens32:
            #dhcp4: true
            addresses:
            - 192.168.0.124/24
            gateway4: 192.168.0.1
            nameservers:
                addresses: [192.168.0.1, 219.146.3.226, 219.146.3.230, 219.146.3.238]
            optional: true
    version: 2
```

> ens32：你的网卡，根据实际情况设置
> dhcp4，gateway4：IPv4协议下的。

- apply
```bash
#netplan apply
```

# Reference
- [network interface configuration](http://manpages.ubuntu.com/manpages/saucy/en/man5/interfaces.5.html)
- [网络配置 - ubuntu](https://help.ubuntu.com/lts/serverguide/network-configuration.html)
- [Ubuntu 18.04修改IP地址 - zhengchaooo](https://blog.csdn.net/zhengchaooo/article/details/80146185)
