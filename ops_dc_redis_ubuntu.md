---
title: "安装redis-server"
description: "Debian-like"
date: 2024-03-10T08:08:08+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

# 1. 安装
Prerequisites:
```
sudo apt install lsb-release curl gpg
```
Add the repository to the apt index:
```
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
```

- install
```bash
sudo apt-get update
sudo apt-get install redis-server
```

## Launch
- local(WSL)
```bash
redis-server
```
- remote(Virtual Box or VM ware etc.)
```bash
sudo redis-server /etc/redis/redis.conf
```

### View port
install netstat:
```
sudo apt install net-tools
```
port:
```
netstat -tunlp
```

### Test
```bash
wsl-1130@DESKTOP-14G6K9S:~$ redis-cli
127.0.0.1:6379> SET mykey2 "This is mykey2"
127.0.0.1:6379> SET mykey3 "This is mykey3"
```

### 有数据
> [redis-command-to-get-all-available-keys](https://stackoverflow.com/questions/5252099/redis-command-to-get-all-available-keys)
```bash
wsl-1130@DESKTOP-14G6K9S:~$ redis-cli
127.0.0.1:6379> KEYS *
1) "mykey3"
2) "mykey2"
127.0.0.1:6379>
```


# 2. 配置
> /etc/redis/redis.conf
```
DENIED Redis is running in protected mode because protected mode is enabled, 
no bind address was specified, no authentication password is requested to clients. 
In this mode connections are only accepted from the loopback interface. 
If you want to connect from external computers to Redis you may adopt one of the following solutions: 
1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' 
 from the loopback interface by connecting to Redis from the same host the server is running, 
however MAKE SURE Redis is not publicly accessible from internet if you do so. 
Use CONFIG REWRITE to make this change permanent. 
2) Alternatively you can just disable the protected mode by editing the Redis configuration file, 
and setting the protected mode option to 'no', and then restarting the server. 
3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 
4) Setup a bind address or an authentication password. 
NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```
## 2.1 配置 standalone
- 本地绑定（关）
```bash
# bind 127.0.0.1 
```

- 保护模式（关）
```
protected-mode no 
```

## 2.2 警告清除
新版本(v7.2.4)已禁止THP:
```
disable-thp yes
```
### Memory
```bash
WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. 
To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
```
A: add vm.overcommit_memory=1
edit sysctl.conf:
```bash
sudo vi /etc/sysctl.conf
```
Take effect:
```bash
sudo sysctl -p
```

### Network
```bash
WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
```
A: add net.core.somaxconn=1024
edit sysctl.conf:
```bash
sudo vi /etc/sysctl.conf
```
Take effect:
```bash
sudo sysctl -p
```

## 2.3 配置cluster
```bash
# Configure redis to use sockets
sudo cp /etc/redis/redis.conf /etc/redis/redis.conf.orig

# Disable Redis listening on TCP by setting 'port' to 0
sed 's/^port .*/port 0/' /etc/redis/redis.conf.orig | sudo tee /etc/redis/redis.conf

# Enable Redis socket for default Debian / Ubuntu path
echo 'unixsocket /var/run/redis/redis.sock' | sudo tee -a /etc/redis/redis.conf

# Grant permission to the socket to all members of the redis group
echo 'unixsocketperm 770' | sudo tee -a /etc/redis/redis.conf

# Create the directory which contains the socket
mkdir /var/run/redis
chown redis:redis /var/run/redis
chmod 755 /var/run/redis

# Activate the changes to redis.conf
sudo service redis-server restart

# Add git to the redis group
sudo usermod -aG redis git
```


# Ref
- [Install on Ubuntu/Debian](https://redis.io/docs/install/install-redis/install-redis-on-linux/)
- [Ubuntu - Disable Transparent Huge Pages](https://techexpert.tips/ubuntu/ubuntu-disable-transparent-huge-pages/)
