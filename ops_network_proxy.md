---
title: "设置proxy"
description: "如何使用代理工具"
date: 2025-04-23T14:08:08+08:00
---

# OS

## Windows
- cmd
```bash
set http_proxy=http://127.0.0.1:8580
set https_proxy=http://127.0.0.1:8580
```
- powershell
```ps
$proxy='http://<IP>:<PORT>'

$ENV:HTTP_PROXY=$proxy
$ENV:HTTPS_PROXY=$proxy
```
ref: https://stackoverflow.com/a/47624713

## Ubuntu
```bash
export http_proxy="http://192.168.1.108:1080"
export https_proxy="http://192.168.1.108:1080"
```

### With user and password
Set up the server in the tool:    
Check "Run xxx as Server(listens 0.0.0.0)"
```bash
export http_proxy=http://USERNAME:PASSWORD@192.168.1.108:8580/
export https_proxy=http://USERNAME:PASSWORD@192.168.1.108:8580/
```

## [FreeBSD](https://nanxiao.gitbooks.io/freebsd-101-hacks/content/posts/configure-proxy.html)
> v12.4
```csh
setenv HTTP_PROXY http://web-proxy.xxxxxx.com:8080
setenv HTTPS_PROXY https://web-proxy.xxxxxx.com:8080
```

# software

## VirtualBox
- Adapter 1    
host only, vboxnet0

- Adapter 2    
NAT

### Guest os
```bash
sudo vi /etc/netplan/00-installer-config.yaml
```

```xml
network:
    version: 2
    renderer: networkd
    ethernets:
        enp0s3:
            addresses:
                - 192.168.56.12/24
            dhcp4: no
        enp0s8:
            dhcp4: no
```


## Wine
```bash
#!/bin/bash

sudo dpkg --add-architecture i386

sudo curl -O https://dl.winehq.org/wine-builds/winehq.key
sudo apt-key add winehq.key

sudo apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ focal main'
sudo apt update

sudo apt install -y --install-recommends winehq-stable
```

### usage wine
```
$ wine --version
$ wine fg798p.exe
```


## git
> .gitconfig
```
[http]
	proxy = http://192.168.43.16:9910
[https]
	proxy = http://192.168.43.16:9910
```
### set proxy
```bash
git config --global http.proxy http://127.0.0.1:8580

git config --global https.proxy https://127.0.0.1:8580
```

### unset
```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## node-js
> [.npmrc](https://github.com/yarnpkg/yarn/issues/3227#issuecomment-389596918)

### npm
```bash
npm config delete https-proxy
npm config set proxy http://host:port
npm config set http-proxy http://host:port
npm config set https-proxy http://host:port
npm config set strict-ssl false
```

#### file
```bash
registry=https://registry.npmjs.org/
proxy=http://127.0.0.1:8580/
http-proxy=http://127.0.0.1:8580
https-proxy=http://127.0.0.1:8580
strict-ssl=false
```

### yarn
- v1
```bash
yarn config set proxy http://host:port
yarn config set https-proxy http://host:port
```
- v2
```bash
yarn config set httpProxy http://host:port
yarn config set httpsProxy http://host:port
```

# Agent Website
国内网站代理
## ruby gems

安装指定版本:[V1.17.3](https://dev.to/st0012/completely-remove-the-default-bundler-from-ci-environment-j0c)
```bash
gem install bundler -v 1.17.3
```

替换掉源地址:gemfile
```bash
https://rubygems.org/
```
变更为:
```bash
https://gems.ruby-china.com/
```

## nodejs
```bash
npm cache clean --force
npm config set registry https://registry.npm.taobao.org
npm config set disturl https://npm.taobao.org/dist
```

## [maven](https://maven.apache.org/guides/mini/guide-proxies.html)
> conf/settings.xml
```xml
  <proxies>
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>127.0.0.1</host>
      <port>8580</port>
    </proxy>
```

# Ref
- [How to set up proxy using http_proxy & https_proxy environment variable in Linux?](https://www.golinuxcloud.com/set-up-proxy-http-proxy-environment-variable/)
- [Install Wine in Ubuntu](https://wiki.winehq.org/Ubuntu)
- [install wine on ubuntu](https://www.tecmint.com/install-wine-on-ubuntu-and-linux-mint/)
