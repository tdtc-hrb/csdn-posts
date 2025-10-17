---
title: "AK100驱动过时问题"
description: "MDKV4.0 - V4.74"
date: 2020-05-04T10:08:08+08:00
---
- IDE：MDKV4.0 - V4.74
- AK100驱动：[V6.0](http://www.zlgmcu.com/download/downs.asp?ID=6405)



现象："..\TKScope\UL2ARM_TKSCP_DRV_ARM_for_AGDI.dll - Driver is outdated !"
![mdk info](https://gitee.com/xiaobin80/csdn/raw/master/images/20140519181217156.png)

解决办法：

1. 删除工程目录中的“TKScope.cfg”

2. 重新设置调试器    
  "Target Options" -> "Debug"
![mdk info](https://gitee.com/xiaobin80/csdn/raw/master/images/20140519181639937.png)
  选择TKScope
![mdk info](https://gitee.com/xiaobin80/csdn/raw/master/images/20140519181750765.png)
  硬件选择（调试器与MCU）
![mdk info](https://gitee.com/xiaobin80/csdn/raw/master/images/20140519181831203.png)
  选择调试器
![mdk info](https://gitee.com/xiaobin80/csdn/raw/master/images/20140519181900625.png)
  选择MCU对应的调试器


其他设置（主要设置、附加设置、TAP设置、程序烧写、初始化宏、硬件自检）请参考“[用户手册](http://www.zlg.cn/sitecn/down.aspx?id=64)”。
