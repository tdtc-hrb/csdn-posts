---
title: "前端（Web）负载均衡"
description: "本系列包括架设Real Server和Keepalived Server、Keepalive安装配置"
date: 2022-09-29T08:38:08+08:00
---
  Nginx's load balancing


# 一、安装
OS: CentOS7 minimal（安装时的选项）
## 0. prepare
```bash
$yum -y install gcc gcc-c++ automake pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

### 1) update OpenSSL
> default directory: /usr/ssl

```bash
$wget https://www.openssl.org/source/openssl-1.1.1q.tar.gz --no-check-certificate
```

### 2) setup
```bash
$./config --prefix=/usr --shared
$make
```
> Root permission Required
```bash
$sudo make install
```

## 1. installation
First, install Nginx of centos repo;    
Then, replace the nginx program file.    
Note: The replaced program file version must be the same as the repo!
```bash
$sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
$sudo yum install -y nginx
```

### 1) down
```bash
$curl --progress https://nginx.org/download/nginx-1.22.0.tar.gz | tar xz
```

### 2) setup module
see arguments:
```bash
$nginx -V
```

#### GD lib
```bash
$sudo yum install gd-devel
```
add param(config):
```bash
--with-http_image_filter_module=dynamic
```

#### libxml2/libxslt
```bash
$sudo yum install libxml2-devel
$sudo yum install libxslt-devel
```
add param(config):
```bash
--with-http_xslt_module=dynamic
```

#### GeoIP library
```bash
$sudo yum install geoip-devel
```
add param(config):
```bash
--with-http_geoip_module=dynamic
```

#### param list
```bash
$ ./configure --prefix=/etc/nginx \
 --sbin-path=/usr/sbin/nginx \
 --modules-path=/usr/lib64/nginx/modules \
 --conf-path=/etc/nginx/nginx.conf \
 --error-log-path=/var/log/nginx/error.log \
 --http-log-path=/var/log/nginx/access.log \
 --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock \
 --http-client-body-temp-path=/var/cache/nginx/client_temp \
 --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
 --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
 --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
 --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
 --user=nginx \
 --group=nginx \
 --with-compat \
 --with-file-aio \
 --with-threads \
 --with-http_addition_module \
 --with-http_auth_request_module \
 --with-http_dav_module \
 --with-http_flv_module \
 --with-http_gunzip_module \
 --with-http_gzip_static_module \
 --with-http_mp4_module \
 --with-http_random_index_module \
 --with-http_realip_module \
 --with-http_secure_link_module \
 --with-http_slice_module \
 --with-http_ssl_module \
 --with-http_stub_status_module \
 --with-http_sub_module \
 --with-http_v2_module \
 --with-mail \
 --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-http_image_filter_module=dynamic --with-http_xslt_module=dynamic --with-http_geoip_module=dynamic --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'
```

### 3) Gen program
```bash
$make
```

Copy nginx and all .so files in the objs folder.
#### nginx
```bash
$sudo cp -rfp objs/nginx /usr/sbin/
```

#### *.so
```bash
$sudo cp objs/*.so /usr/lib64/nginx/modules/
```

## 2. fw
> port 80

```bash
$sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
```

## 3. run
- start
```bash
$sudo /usr/sbin/nginx
```

- restart
```bash
$sudo /usr/sbin/nginx -s reload
```


# 二、配置
## 0. load lib
> /etc/nginx/nginx.conf
add add below in the main context:
```bash
load_module "modules/ngx_http_geoip_module.so";
load_module "modules/ngx_http_image_filter_module.so";
load_module "modules/ngx_http_xslt_filter_module.so";
```

## 1. load balancing
Official website recommended configuration changes

### official
- /etc/nginx/nginx.conf
```bash
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }
}
```

- /etc/nginx/conf.d/default.conf
```bash
server {
    listen 80;

    location / {
        proxy_pass http://myapp1;
    }
}
```

### alter
> Our own configuration
- /etc/nginx/nginx.conf
```bash
http {
    upstream localhost {
        server 192.168.0.122:8080 weight=6; ## local
        server 192.168.0.127:8080 weight=3;
    }
}
```

- /etc/nginx/conf.d/default.conf
```bash
server {
    listen 80;

    location / {
        proxy_connect_timeout 3;
        proxy_send_timeout 30;
        proxy_read_timeout 30;
        proxy_pass http://localhost;
    }
}
```

# Reference
- [nginx - setup](https://nginx.org/en/docs/configure.html)
- [nginx - load balancing](https://nginx.org/en/docs/http/load_balancing.html)
- [搭建Keepalived + Nginx + Tomcat的高可用负载均衡架构 - feinik](https://my.oschina.net/feinik/blog/1590941)
- [yum安装的nginx新增模块http_image_filter_module笔记](https://www.cnblogs.com/zqsb/p/11388889.html)

---
# FAQ
## 1. nginx - nginx: [emerg] bind() to [::]:80 failed (98: Address already in use) [closed]
在配置文件中ipv4与ipv6都使用80端口

解决办法：
### 1）更改端口
```bash
listen 0.0.0.0:80 default_server;
listen [::]:81 default_server;
```

### 2）只使用一种协议
> 参考 [nginx-nginx-emerg-bind-to-80-failed-98-address-already-in-use](https://stackoverflow.com/a/15101745)
```bash
listen 0.0.0.0:80 default_server;
listen [::]:80 ipv6only=on default_server;
```

我的解决办法，不使用IPv6。
```bash
listen 0.0.0.0:80 default_server;
## listen [::]:80 default_server;
```
