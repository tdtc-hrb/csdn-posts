---
title: "架设VSFTP服务器"
description: "ops系列-部门服务器(Ubuntu)"
date: 2020-04-13T03:08:08+08:00
---
> Ubuntu版本：14.04
参考Ubuntu14.04的[FTP Server文档](https://help.ubuntu.com/14.04/serverguide/ftp-server.html)

# 安装vsftpd
```bash
root@tdtc010-Vostro-270:~# apt-get install vsftpd
```

# 配置

## 允许上传
```bash
local_enable=YES;
write_enable=YES;
```

## 允许匿名上传
```bash
anon_upload_enable=YES;
```



# FAQ
## 553 Could not create file
![2013-04-19 23:13:47](https://gitee.com/xiaobin80/csdn/raw/master/images/1366383847_4148.png)
执行“快速连接”;提示错误信息：553 Could not create file

### 新建一个目录
```bash
root@tdtc010-Vostro-270:/srv/ftp# mkdir upload_xp
```

### 更改目录权限
默认权限（FileZilla右击文件属性）
![2013-04-19 23:13:47](https://gitee.com/xiaobin80/csdn/raw/master/images/20170902112233231.png)
更改为可读写：
```bash
root@tdtc010-Vostro-270:/srv/ftp# chmod 777 upload_xp
```
![2013-04-19 23:13:47](https://gitee.com/xiaobin80/csdn/raw/master/images/20170902112934299.png)


## 550 Failed to open file
解决办法： 开启公共权限（读）
```bash
chmod 644 myfile.iso
```

## 显示ftp目录信息
在目录下建立一个.message文件即可。     
/srv/ftp/upload_xp/.message    
文件内容：    
```bash
欢迎使用VSFTPD，这个是upload_xp目录
```


再次，在FileZilla执行上传，显示成功：
```bash
状态:	已连接
状态:	开始上传 D:\360Downloads\perl-5.16.3.tar.gz
命令:	CWD /upload_xp
响应:	250 Directory successfully changed.
命令:	TYPE I
响应:	200 Switching to Binary mode.
命令:	PASV
响应:	227 Entering Passive Mode (192,168,1,101,232,175).
命令:	STOR perl-5.16.3.tar.gz
响应:	150 Ok to send data.
响应:	226 Transfer complete.
状态:	文件传输成功，传输了 16,930,885 字节 (用时58 秒)
```
全文完。

#  附

## 1. 终端操作过程（仅供参考）

```bash
tdtc010@tdtc010-Vostro-270:~$ su - root
Password:
root@tdtc010-Vostro-270:~# apt-get install vsftpd
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  vsftpd
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 124 kB of archives.
After this operation, 342 kB of additional disk space will be used.
Get:1 http://cn.archive.ubuntu.com/ubuntu/ precise/main vsftpd amd64 2.3.5-1ubuntu2 [124 kB]
Fetched 124 kB in 0s (153 kB/s)
Preconfiguring packages ...
Selecting previously unselected package vsftpd.
(Reading database ... 168616 files and directories currently installed.)
Unpacking vsftpd (from .../vsftpd_2.3.5-1ubuntu2_amd64.deb) ...
Processing triggers for man-db ...
Processing triggers for ureadahead ...
ureadahead will be reprofiled on next reboot
Setting up vsftpd (2.3.5-1ubuntu2) ...
vsftpd start/running, process 2519
root@tdtc010-Vostro-270:~# cd /srv/ftp
root@tdtc010-Vostro-270:/srv/ftp# ls
root@tdtc010-Vostro-270:/srv/ftp# /etc/init.d/vsftpd restart
Rather than invoking init scripts through /etc/init.d, use the service(8)
utility, e.g. service vsftpd restart

Since the script you are attempting to invoke has been converted to an
Upstart job, you may also use the stop(8) and then start(8) utilities,
e.g. stop vsftpd ; start vsftpd. The restart(8) utility is also available.
vsftpd stop/waiting
vsftpd start/running, process 2566
root@tdtc010-Vostro-270:/srv/ftp# gedit /etc/vsftpd.conf

(gedit:2574): GLib-GIO-WARNING **: Missing callback called fullpath = /root/.local/share/recently-used.xbel

root@tdtc010-Vostro-270:/srv/ftp# /etc/init.d/vsftpd restart
Rather than invoking init scripts through /etc/init.d, use the service(8)
utility, e.g. service vsftpd restart

Since the script you are attempting to invoke has been converted to an
Upstart job, you may also use the stop(8) and then start(8) utilities,
e.g. stop vsftpd ; start vsftpd. The restart(8) utility is also available.
vsftpd stop/waiting
vsftpd start/running, process 2631
root@tdtc010-Vostro-270:/srv/ftp# gedit /etc/vsftpd.conf
root@tdtc010-Vostro-270:/srv/ftp# /etc/init.d/vsftpd restart
Rather than invoking init scripts through /etc/init.d, use the service(8)
utility, e.g. service vsftpd restart

Since the script you are attempting to invoke has been converted to an
Upstart job, you may also use the stop(8) and then start(8) utilities,
e.g. stop vsftpd ; start vsftpd. The restart(8) utility is also available.
vsftpd stop/waiting
vsftpd start/running, process 2657
root@tdtc010-Vostro-270:/srv/ftp# ls
root@tdtc010-Vostro-270:/srv/ftp# mkdir upload_xp
root@tdtc010-Vostro-270:/srv/ftp# ls
upload_xp
root@tdtc010-Vostro-270:/srv/ftp# chown ftp:root /srv/ftp/upload_xp
root@tdtc010-Vostro-270:/srv/ftp#
```

## 2. 配置文件(/etc/vsftpd.conf)
```bash
# Example config file /etc/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
#
# Run standalone?  vsftpd can run either from an inetd or as a standalone
# daemon started from an initscript.
listen=YES
#
# Run standalone with IPv6?
# Like the listen parameter, except vsftpd will listen on an IPv6 socket
# instead of an IPv4 one. This parameter and the listen parameter are mutually
# exclusive.
#listen_ipv6=YES
#
# Allow anonymous FTP? (Beware - allowed by default if you comment this out).
anonymous_enable=YES
#
# Uncomment this to allow local users to log in.
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
#local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
use_localtime=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
#chroot_local_user=YES
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
#chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd.chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# Customization
#
# Some of vsftpd's settings don't fit the filesystem layout by
# default.
#
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
#
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
#
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
rsa_cert_file=/etc/ssl/private/vsftpd.pem
```
