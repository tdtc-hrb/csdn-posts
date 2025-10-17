---
title: "开启sudo"
description: "RHEL and Debian"
date: 2024-03-20T00:08:08+08:00
---


# new user
```bash
#useradd tdtc
#passwd tdtc
```

# Append group

## Modify su file
- CentOS 6
v6 and below versions

### Open wheel
```bash
#vi /etc/pam.d/su
```

```bash
auth required /lib/security/$ISA/pam_wheel.so use_uid
```

### Attachment of group members

```bash
#gpasswd -a tdtc wheel
```

## Command
- CentOS 7/8
- Debian    
Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

### CentOS 7/8
> CentOS8 needs to be executed locally,
Unable to execute on ssh network side.
```bash
#usermod –aG wheel tdtc
```

### Debian
```
adduser tdtc sudo
```

# test
Query all users with sudo permissions.

```bash
[tdtc@tdtc102 ~]$ sudo lid -g wheel

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tdtc:
 tdtc(uid=1000)
[tdtc@tdtc102 ~]$
```

# 参考
- [usermod -g和gpasswd -a的区别](https://blog.csdn.net/weixin_37959457/article/details/80759374)
- [Configure sudo in Debian](https://documentation.arcserve.com/Arcserve-UDP/Available/V6.5/ENU/Bookshelf_Files/HTML/Agent%20Online%20Help%20Linux/Content/AgentforLinuxUserGuide/udpl_config_sudo_debian.htm)
