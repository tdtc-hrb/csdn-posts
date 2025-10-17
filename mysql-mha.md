---
title: "主备切换 - MySQL+MHA"
description: "本系列包括MySQL 主从复制"
date: 2022-09-27T00:38:08+08:00
---
> 在进行安装配置前，请确认已经完成了 [MySQL 主从复制](https://tdtc-hrb.github.io/csdn/post/mysql-master-slave-replication/)

- MHA
> (V0.58)
1. [Manager](https://github.com/yoshinorim/mha4mysql-manager)
2. [Node](https://github.com/yoshinorim/mha4mysql-node)

>> baidu yun - backup
1. [Mha manager](https://pan.baidu.com/s/1FBX1yI4HncdI9UZCdzdcHQ)
2. [Mha node](https://pan.baidu.com/s/1wA5NOUgjnDDJxQTL0U-1DA)

> 安装前准备 [ssh相互免密](https://tdtc-hrb.github.io/csdn/post/ops_ssh_without_password/)

![sql status](https://github.com/tdtc-hrb/csdn/raw/master/images/wuhan2017120193.png)


# 一、安装
|Name|IP|Memo|
| - | - | - |
|mha1|192.168.0.151|Monitor host

 使用pscp上传文件请参考 [MySQL 主从复制](https://tdtc-hrb.github.io/csdn/post/mysql-master-slave-replication/) - 5. test data

## 1. MHA Node
 > 所有MySQL服务器（mysql1、mysql11、mysql12）和MHA 管理机（MHA）都要安装。

 ```bash
sudo yum localinstall mha4mysql-node-0.58-0.el7.centos.noarch.rpm
 ```

## 2. MHA Manager
> CentOS源有perl-Config-Tiny。

```bash
sudo yum install perl-Config-Tiny
```

> 安装[epel](https://fedoraproject.org/wiki/EPEL)，
可以找到[perl-Log-Dispatch、perl-Parallel-ForkManager](https://github.com/yoshinorim/mha4mysql-manager/wiki/Installation#installing-mha-manager)。

```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install perl-Log-Dispatch perl-Parallel-ForkManager -y
```
> 安装manager

```bash
sudo yum localinstall mha4mysql-manager-0.58-0.el7.centos.noarch.rpm
```

## 3. MySQL主从配置
> （补充部分）

### 1) repl user
>> Slave Server（mysql11, mysql12）

```sql
mysql> CREATE USER 'repl'@'192.168.0.%' IDENTIFIED BY 'qaz1!Xsw2@';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.0.%';
```

### 2) add log-bin
> /etc/my.cnf

>> Slave Server(mysql11,mysql12)

```bash
log-bin=mysql161-bin
```

# 二、配置

## 1. Manager
### 1) check_repl_delay
> 设置复制延时为0和candidate_master=1，会强制切换master。

```bash
check_repl_delay=0
```

### 2）binlog
```bash
master_binlog_dir=/var/lib/mysql/
remote_workdir=/tmp
```

### 3）发通知
```bash
report_script=/usr/bin/send_report
```


## 2. Node

### 1) add user
> (all mysql)

```sql
mysql> CREATE USER 'manager'@'192.168.0.%' identified by 'qaz1!Xsw2@';
mysql> grant all privileges on *.* to 'manager'@'192.168.0.%';

mysql> flush  privileges;
```


# 三、Test

```bash
sudo masterha_check_repl --conf=/etc/masterha/app1.cnf
```

> Info:

```bash
[tdtc@mha1 ~]$ sudo masterha_check_repl --conf=/etc/masterha/app1.cnf
Wed Feb 27 12:42:36 2019 - [warning] Global configuration file /etc/masterha_default.cnf not found. Skipping.
Wed Feb 27 12:42:36 2019 - [info] Reading application default configuration from /etc/masterha/app1.cnf..
Wed Feb 27 12:42:36 2019 - [info] Reading server configuration from /etc/masterha/app1.cnf..
Wed Feb 27 12:42:36 2019 - [info] MHA::MasterMonitor version 0.58.
Wed Feb 27 12:42:37 2019 - [info] GTID failover mode = 0
Wed Feb 27 12:42:37 2019 - [info] Dead Servers:
Wed Feb 27 12:42:37 2019 - [info] Alive Servers:
Wed Feb 27 12:42:37 2019 - [info]   192.168.0.160(192.168.0.160:3306)
Wed Feb 27 12:42:37 2019 - [info]   192.168.0.161(192.168.0.161:3306)
Wed Feb 27 12:42:37 2019 - [info]   192.168.0.162(192.168.0.162:3306)
Wed Feb 27 12:42:37 2019 - [info] Alive Slaves:
Wed Feb 27 12:42:37 2019 - [info]   192.168.0.161(192.168.0.161:3306)  Version=5.7.25-log (oldest major version between slaves) log-bin:enabled
Wed Feb 27 12:42:37 2019 - [info]     Replicating from 192.168.0.160(192.168.0.160:3306)
Wed Feb 27 12:42:37 2019 - [info]     Primary candidate for the new Master (candidate_master is set)
Wed Feb 27 12:42:37 2019 - [info]   192.168.0.162(192.168.0.162:3306)  Version=5.7.25-log (oldest major version between slaves) log-bin:enabled
Wed Feb 27 12:42:37 2019 - [info]     Replicating from 192.168.0.160(192.168.0.160:3306)
Wed Feb 27 12:42:37 2019 - [info]     Not candidate for the new Master (no_master is set)
Wed Feb 27 12:42:37 2019 - [info] Current Alive Master: 192.168.0.160(192.168.0.160:3306)
Wed Feb 27 12:42:37 2019 - [info] Checking slave configurations..
Wed Feb 27 12:42:37 2019 - [info]  read_only=1 is not set on slave 192.168.0.161(192.168.0.161:3306).
Wed Feb 27 12:42:37 2019 - [warning]  relay_log_purge=0 is not set on slave 192.168.0.161(192.168.0.161:3306).
Wed Feb 27 12:42:37 2019 - [info]  read_only=1 is not set on slave 192.168.0.162(192.168.0.162:3306).
Wed Feb 27 12:42:37 2019 - [warning]  relay_log_purge=0 is not set on slave 192.168.0.162(192.168.0.162:3306).
Wed Feb 27 12:42:37 2019 - [info] Checking replication filtering settings..
Wed Feb 27 12:42:37 2019 - [info]  binlog_do_db= , binlog_ignore_db=
Wed Feb 27 12:42:37 2019 - [info]  Replication filtering check ok.
Wed Feb 27 12:42:37 2019 - [info] GTID (with auto-pos) is not supported
Wed Feb 27 12:42:37 2019 - [info] Starting SSH connection tests..
Wed Feb 27 12:42:41 2019 - [info] All SSH connection tests passed successfully.
Wed Feb 27 12:42:41 2019 - [info] Checking MHA Node version..
Wed Feb 27 12:42:42 2019 - [info]  Version check ok.
Wed Feb 27 12:42:42 2019 - [info] Checking SSH publickey authentication settings on the current master..
Wed Feb 27 12:42:42 2019 - [info] HealthCheck: SSH to 192.168.0.160 is reachable.
Wed Feb 27 12:42:42 2019 - [info] Master MHA Node version is 0.58.
Wed Feb 27 12:42:42 2019 - [info] Checking recovery script configurations on 192.168.0.160(192.168.0.160:3306)..
Wed Feb 27 12:42:42 2019 - [info]   Executing command: save_binary_logs --command=test --start_pos=4 --binlog_dir=/var/lib/mysql/ --output_file=/tmp/save_binary_logs_test --manager_version=0.58 --start_file=mysql160-bin.000003
Wed Feb 27 12:42:42 2019 - [info]   Connecting to root@192.168.0.160(192.168.0.160:22)..
  Creating /tmp if not exists..    ok.
  Checking output directory is accessible or not..
   ok.
  Binlog found at /var/lib/mysql/, up to mysql160-bin.000003
Wed Feb 27 12:42:43 2019 - [info] Binlog setting check done.
Wed Feb 27 12:42:43 2019 - [info] Checking SSH publickey authentication and checking recovery script configurations on all alive slave servers..
Wed Feb 27 12:42:43 2019 - [info]   Executing command : apply_diff_relay_logs --command=test --slave_user='manager' --slave_host=192.168.0.161 --slave_ip=192.168.0.161 --slave_port=3306 --workdir=/tmp --target_version=5.7.25-log --manager_version=0.58 --relay_log_info=/var/lib/mysql/relay-log.info  --relay_dir=/var/lib/mysql/  --slave_pass=xxx
Wed Feb 27 12:42:43 2019 - [info]   Connecting to root@192.168.0.161(192.168.0.161:22)..
  Checking slave recovery environment settings..
    Opening /var/lib/mysql/relay-log.info ... ok.
    Relay log found at /var/lib/mysql, up to mysql11-relay-bin.000002
    Temporary relay log file is /var/lib/mysql/mysql11-relay-bin.000002
    Checking if super_read_only is defined and turned on.. not present or turned off, ignoring.
    Testing mysql connection and privileges..
mysql: [Warning] Using a password on the command line interface can be insecure.
 done.
    Testing mysqlbinlog output.. done.
    Cleaning up test file(s).. done.
Wed Feb 27 12:42:43 2019 - [info]   Executing command : apply_diff_relay_logs --command=test --slave_user='manager' --slave_host=192.168.0.162 --slave_ip=192.168.0.162 --slave_port=3306 --workdir=/tmp --target_version=5.7.25-log --manager_version=0.58 --relay_log_info=/var/lib/mysql/relay-log.info  --relay_dir=/var/lib/mysql/  --slave_pass=xxx
Wed Feb 27 12:42:43 2019 - [info]   Connecting to root@192.168.0.162(192.168.0.162:22)..
  Checking slave recovery environment settings..
    Opening /var/lib/mysql/relay-log.info ... ok.
    Relay log found at /var/lib/mysql, up to mysql12-relay-bin.000002
    Temporary relay log file is /var/lib/mysql/mysql12-relay-bin.000002
    Checking if super_read_only is defined and turned on.. not present or turned off, ignoring.
    Testing mysql connection and privileges..
mysql: [Warning] Using a password on the command line interface can be insecure.
 done.
    Testing mysqlbinlog output.. done.
    Cleaning up test file(s).. done.
Wed Feb 27 12:42:44 2019 - [info] Slaves settings check done.
Wed Feb 27 12:42:44 2019 - [info]
192.168.0.160(192.168.0.160:3306) (current master)
 +--192.168.0.161(192.168.0.161:3306)
 +--192.168.0.162(192.168.0.162:3306)

Wed Feb 27 12:42:44 2019 - [info] Checking replication health on 192.168.0.161..
Wed Feb 27 12:42:44 2019 - [info]  ok.
Wed Feb 27 12:42:44 2019 - [info] Checking replication health on 192.168.0.162..
Wed Feb 27 12:42:44 2019 - [info]  ok.
Wed Feb 27 12:42:44 2019 - [warning] master_ip_failover_script is not defined.
Wed Feb 27 12:42:44 2019 - [warning] shutdown_script is not defined.
Wed Feb 27 12:42:44 2019 - [info] Got exit code 0 (Not master dead).

MySQL Replication Health is OK.
[tdtc@mha1 ~]$
```

# 附件

## 1. send_report

```bash
#!/usr/bin/perl

#  Copyright (C) 2011 DeNA Co.,Ltd.
#
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation; either version 2 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#   along with this program; if not, write to the Free Software
#  Foundation, Inc.,
#  51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA

## Note: This is a sample script and is not complete. Modify the script based on your environment.

use strict;
use warnings FATAL => 'all';
use Mail::Sender;
use Getopt::Long;

#new_master_host and new_slave_hosts are set only when recovering master succeeded
my ( $dead_master_host, $new_master_host, $new_slave_hosts, $subject, $body );
my $smtp='smtp.163.com';
my $mail_from='love_shidie@163.com';
my $mail_user='love_shidie';
my $mail_pass='xxxxx';
my $mail_to=['veic_2005@163.com','sjzligb@126.com'];
GetOptions(
  'orig_master_host=s' => \$dead_master_host,
  'new_master_host=s'  => \$new_master_host,
  'new_slave_hosts=s'  => \$new_slave_hosts,
  'subject=s'          => \$subject,
  'body=s'             => \$body,
);

mailToContacts($smtp,$mail_from,$mail_user,$mail_pass,$mail_to,$subject,$body);

sub mailToContacts {
    my ( $smtp, $mail_from, $user, $passwd, $mail_to, $subject, $msg ) = @_;
    open my $DEBUG, "> /tmp/monitormail.log"
        or die "Can't open the debug      file:$!\n";
    my $sender = new Mail::Sender {
        ctype       => 'text/plain; charset=utf-8',
        encoding    => 'utf-8',
        smtp        => $smtp,
        from        => $mail_from,
        auth        => 'LOGIN',
        TLS_allowed => '0',
        authid      => $user,
        authpwd     => $passwd,
        to          => $mail_to,
        subject     => $subject,
        debug       => $DEBUG
    };

    $sender->MailMsg(
        {   msg   => $msg,
            debug => $DEBUG
        }
    ) or print $Mail::Sender::Error;
    return 1;
}



# Do whatever you want here

exit 0;
```

## 2. [app1.cnf](https://pan.baidu.com/s/13dWV0oH1coGxtUYD8wcDmA)
> /etc/masterha/app1.cnf

```bash
[server default]
user=manager
password=qaz1!Xsw2@
ssh_user=root
manager_workdir=/var/log/masterha/app1
manager_log=/var/log/masterha/app1/manager.log

check_repl_delay=0

master_binlog_dir=/var/lib/mysql/
remote_workdir=/tmp

#master_ip_failover_script= /usr/bin/master_ip_failover
#master_ip_online_change_script= /usr/bin/master_ip_online_change

repl_password=qaz1!Xsw2@
repl_user=repl

report_script=/usr/bin/send_report

[server1]
hostname=192.168.0.160
port=3306
candidate_master=1

[server2]
hostname=192.168.0.161
port=3306
candidate_master=1
check_repl_delay=0

[server3]
hostname=192.168.0.162
port=3306
no_master=1
```
