---
title: "grub救援模式"
description: "Ubuntu进入grub的操作"
date: 2023-03-15T00:19:08+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

出现“grub rescue>”提示符；也称grub救援模式。

# 1. 设置引导
> (hd?,?)：安装的引导分区     
在我的机器是(hd0, msdos3)

```bash
grub rescue>ls
grub rescue>ls (hd?,?)/boot/grub
grub rescue>set root(hd?,?)
grub rescue>set prefix=(hd?,?)/boot/grub
```

## insmod
one:
```bash
insmod /boot/grub/normal.mod
/boot/grub/normal
```

two:
```bash
insmod normal
normal
```

# 2. 更新grub
在终端上执行：
```bash
sudo grub-install /dev/sda
sudo update-grub
```
