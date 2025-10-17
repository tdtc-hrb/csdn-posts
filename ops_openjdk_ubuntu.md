---
title: "设置OpenJDK"
description: "ubuntu"
date: 2024-03-15T07:08:08+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

> ubuntu 20.04.2

```bash
wsl1017@DESKTOP-14G6K9S:~$ java -version

Command 'java' not found, but can be installed with:

sudo apt install default-jre
sudo apt install openjdk-11-jre-headless
sudo apt install openjdk-8-jre-headless
```

# installation

1. install jre
```bash
wsl1017@DESKTOP-14G6K9S:/tmp$ sudo apt-get install openjdk-8-jre-headless
```

2. install jdk
```bash
sudo apt-get install openjdk-8-jdk-headless
```

3. set JAVA home
```bash
sudo vi /etc/profile
```

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

## [ubuntu version](https://askubuntu.com/a/944260)
The very short answer is that OpenJDK 8 as of 2017-08-08 is not officially available for Ubuntu 14.04.

Upgrade to a newer version of Ubuntu. OpenJDK 8 is available from 14.10 and onwards.
- Ubuntu 14.04    
OpenJDK 7
- Ubuntu 16.04    
OpenJDK 9
- Ubuntu 18.04    
OpenJDK 11


# Reference
- [How to Install OpenJDK JAVA 11/8 in Ubuntu and Debian](https://tecadmin.net/install-openjdk-java-ubuntu/)
