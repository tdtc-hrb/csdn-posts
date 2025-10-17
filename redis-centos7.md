---
title: "在RHEL/CentOS安装Redis"
date: 2022-06-11T04:58:39+08:00
tags: ["Redis", "CentOS7", "Development Tools", "wget", "RHEL"]
---

# 0. preinstall

- User set
```bash
$ useradd tdtc
$ passwd tdtc
```

- [Yum Repo](https://tdtc-hrb.github.io/csdn/post/ops_rhel_repo/)


## 1) yum install
```bash
$sudo yum groupinstall "Development Tools"
$sudo yum install tcl wget
```

### v6+
Just to clarify: there is no way this issue will be fixed, Redis >= 6 will require a C11 compiler.
```bash
sudo yum install centos-release-scl
sudo yum install devtoolset-8
scl enable devtoolset-8 bash
```

## 2) verification
```bash
$gcc -v
$make -v
$echo 'puts [info patchlevel];exit 0' | tclsh
```


# 1. installation

## 1) install
```bash
$wget https://download.redis.io/releases/redis-6.0.16.tar.gz
$tar xzvf redis-6.0.16.tar.gz
$cd redis-6.0.16
$make
## Single Core
$taskset -c 0 make test
## sudo
$sudo make install
```

## 2) config
```bash
$sudo mkdir /etc/redis
$sudo mkdir -p /var/redis
$sudo cp /home/tdtc/redis-6.0.16/redis.conf /etc/redis
```

### redis.conf
```bash
port  6379				         #default port is already 6379.
daemonize yes			         #run as a daemon
supervised systemd			     #signal systemd
pidfile /var/run/redis_6379.pid  #specify pid file
loglevel notice			         #server verbosity level
logfile /var/log/redis.log		 #log file name
dir  /var/redis/		         #redis directory
```



# 2. command
You do not need to execute the following commands when you are not maintaining it, only the first time.

## redis.service

### v6-
```bash
$sudo vi /etc/systemd/system/redis.service
```

```bash
[Unit]
Description=Redis In-Memory Data Store
After=network.target
[Service]
User=root
Group=root
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always
Type=Forking
LimitNOFILE=10032
[Install]
WantedBy=multi-user.target
```

### v6+
> /redis-6.0.16/utils/systemd-redis_server.service
```
[Unit]
Description=Redis Server v6
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
LimitNOFILE=10032
NoNewPrivileges=yes
Type=forking
UMask=0077

[Install]
WantedBy=multi-user.target

```

## redis service
```bash
$sudo systemctl start redis
$sudo systemctl enable redis
$sudo systemctl status redis
```

# 3. test
![win cmd](https://github.com/tdtc-hrb/csdn/raw/master/images/20180902055948739.png)

# Reference
- [How to Install Redis Server in CentOS and Debian Based Systems](https://www.tecmint.com/install-redis-server-in-centos-ubuntu-debian/)
- [Build fails under GCC](https://github.com/redis/redis/issues/6286)
- [Developer Toolset 8](https://www.softwarecollections.org/en/scls/rhscl/devtoolset-8/)
- [Centos8 安装 Redis6.0.16](https://www.macnp.com/express/info/8134d10f)
