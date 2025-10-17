---
title: "dpkg的info问题和file system只读问题"
date: 2022-04-10T02:10:39+08:00
---

# Install
```bash
sudo dpkg -i debName.deb
```

# issues

## info问题

```bash
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

解决办法：
```bash
cd /var/lib/dpkg
sudo mv info info.bak
sudo mkdir info
```


## 文件系统只读问题

```bash
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (30: Read-only file system)
```

```bash
sudo fsck -Af
sudo reboot
```
