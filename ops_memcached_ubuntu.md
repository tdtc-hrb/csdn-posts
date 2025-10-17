---
title: "安装memcached"
description: "ubuntu"
date: 2024-12-11T09:08:08+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

> VMware: Ubuntu 20.04.2

# Install and update
```bash
#sudo apt-get install memcached [0]
#sudo apt-get install build-essential
#sudo apt-get install libevent-dev*
#wget http://memcached.org/files/memcached-1.6.33.tar.gz
#tar -zxvf memcached-1.6.33.tar.gz
#cd memcached-1.6.33
#./configure && make && sudo make install
```

# Start
## allow port
```bash
sudo ufw allow 11211
```

## config
```bash
sudo vi /etc/memcached.conf
```
修改为服务器的IP:
```bash
# Specify which IP address to listen on. The default is to listen on all IP addresses
# This parameter is one of the only security measures that memcached has, so make sure
# it's listening on a firewalled interface.
-l 192.168.0.103
```

## run
```bash
$ sudo service memcached start
```

### view
```bash
ps -ef|grep memcached
```

# Reference
- 0. default version
```bash
$ memcached -V
memcached 1.5.22
```
