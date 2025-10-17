---
title: " 使用脚本安装gitlab"
date: 2020-04-10T02:15:39+08:00
---

# 一、安装

## 1. depend
```bash
$sudo apt-get update
$sudo apt-get install ca-certificates curl openssh-server postfix vim
```

## 2. down script
```bash
$curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
```

## 3. exec script
```bash
$sudo bash ~/script.deb.sh
```

## 4. install
If it is "qiang", please see 《代理set - ubuntu》

```bash
$sudo apt-get install gitlab-ce
```

## 5. config

### 1) ufw
```bash
$sudo ufw allow http
$sudo ufw allow https
$sudo ufw allow OpenSSH
```

### 2) gitlab.rb

```bash
$sudo vim /etc/gitlab/gitlab.rb
```

```bash
# If your GitLab server does not have a domain name, you will need to use an IP
# address instead of a domain and keep the protocol as `http`.
external_url 'http://localhost'
```

### 3) exec gitlab-ctl

```bash
$sudo gitlab-ctl reconfigure
```

## 6. run
http://your_ip

- set password for root

- login for root


# 二、管理

## 1. 控制

### 1）查看
```bash
sudo gitlab-ctl status
```

### 2）开始、停止和重启
```bash
sudo gitlab-ctl start
sudo gitlab-ctl stop
sudo gitlab-ctl restart
```


## 2. 备份/恢复
default path:     
/var/opt/gitlab/backups

### 1）备份
```bash
sudo gitlab-rake gitlab:backup:create
```

### 2）恢复

#### （1）停止数据服务
```bash
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
```

### （2）恢复指定
```bash
sudo gitlab-rake gitlab:backup:restore BACKUP=1568533129_2019_09_15_12.2.5
```

# Reference
- [《How To Install and Configure GitLab on Ubuntu 16.04》](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-gitlab-on-ubuntu-16-04)
