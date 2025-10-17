---
title: "在阿里云上部署httpd"
description: "ops系列-云服务器(centos7)"
date: 2020-04-13T01:08:08+08:00
---

> Server：ali云服务器ECS    
OS: CentOS 6/7 (x64)
httpd路径: /opt/httpd

两种不同的init系统部署方式
========================

# 一、SysVinit
SysVinit是CentOS6的默认1号进程。
## 1. Copy script
```bash
# cp /opt/httpd/bin/apachectl /etc/rc.d/init.d/httpd
```

## 2. Edit script
```bash
# cd /etc/rc.d/init.d
# vi httpd
```

The original file(apachectl)
```bash
#!/bin/sh
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
#
# Apache control script designed to allow an easy command line interface
# to controlling Apache.  Written by Marc Slemko, 1997/08/23
#
# The exit codes returned are:
#   XXX this doc is no longer correct now that the interesting
#   XXX functions are handled by httpd
#	0 - operation completed successfully
#	1 -
#	2 - usage error
#	3 - httpd could not be started
#	4 - httpd could not be stopped
#	5 - httpd could not be started during a restart
#	6 - httpd could not be restarted during a restart
#	7 - httpd could not be restarted during a graceful restart
#	8 - configuration syntax error
#
# When multiple arguments are given, only the error from the _last_
# one is reported.  Run "apachectl help" for usage info
#
ARGV="$@"
#
# |||||||||||||||||||| START CONFIGURATION SECTION  ||||||||||||||||||||
# --------------------                              --------------------
#
# the path to your httpd binary, including options if necessary
HTTPD='/opt/httpd/bin/httpd'
#
# pick up any necessary environment variables
if test -f /opt/httpd/bin/envvars; then
  . /opt/httpd/bin/envvars
fi
#
# a command that outputs a formatted text version of the HTML at the
# url given on the command line.  Designed for lynx, however other
# programs may work.  
LYNX="lynx -dump"
#
# the URL to your server's mod_status status page.  If you do not
# have one, then status and fullstatus will not work.
STATUSURL="http://localhost:80/server-status"
#
# Set this variable to a command that increases the maximum
# number of file descriptors allowed per child process. This is
# critical for configurations that use many file descriptors,
# such as mass vhosting, or a multithreaded server.
ULIMIT_MAX_FILES="ulimit -S -n `ulimit -H -n`"
# --------------------                              --------------------
# ||||||||||||||||||||   END CONFIGURATION SECTION  ||||||||||||||||||||

# Set the maximum number of file descriptors allowed per child process.
if [ "x$ULIMIT_MAX_FILES" != "x" ] ; then
    $ULIMIT_MAX_FILES
fi

ERROR=0
if [ "x$ARGV" = "x" ] ; then
    ARGV="-h"
fi

case $ARGV in
start|stop|restart|graceful|graceful-stop)
    $HTTPD -k $ARGV
    ERROR=$?
    ;;
startssl|sslstart|start-SSL)
    echo The startssl option is no longer supported.
    echo Please edit httpd.conf to include the SSL configuration settings
    echo and then use "apachectl start".
    ERROR=2
    ;;
configtest)
    $HTTPD -t
    ERROR=$?
    ;;
status)
    $LYNX $STATUSURL | awk ' /process$/ { print; exit } { print } '
    ;;
fullstatus)
    $LYNX $STATUSURL
    ;;
*)
    $HTTPD $ARGV
    ERROR=$?
esac

exit $ERROR
```

 Add the following section to the file:(File header)
```bash
#!/bin/sh
#
# Startup script for the Apache Web Server
#
# chkconfig: 345 85 15
# description: Apache is a World Wide Web server.  It is used to serve \
#              HTML files and CGI.
# processname: httpd
```


# 二、SystemD
SystemD是CentOS7的默认1号进程。

## 1. install
```bash
# yum install httpd
```

## 2. start
```bash
systemctl enable httpd.service
systemctl start httpd.service
```

## 3. Test
```bash
/opt/httpd/bin/apachectl -v
```
```bash
Server version: Apache/2.2.27 (Unix)
Server built:   Apr  5 2014 00:42:59
```

# Reference
- [chkconfig](http://www.redhat.com/archives/redhat-list/2001-March/msg01309.html)
> "service httpd does not support chkconfig" solutions

- [apachectl](http://httpd.apache.org/docs/2.2/programs/apachectl.html)(en)

- [apachectl](http://www.jinbuguo.com/apache/menu22/programs/apachectl.html)(zh)

- [Linux Systemd - Start/Stop/Restart Services in RHEL / CentOS 7](https://linoxide.com/linux-command/start-stop-services-systemd/)
