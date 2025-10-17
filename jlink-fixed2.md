---
title: "clone问题"
description: "J-Link两篇之二"
date: 2020-05-05T10:08:08+08:00
---

在J-Link驱动自动升级后，会出现下面情况。

The connected emulator is a j-link clone.
![error info](https://gitee.com/xiaobin80/csdn/raw/master/images/132134001292220.png)

# 修改固件
> 使用UltraEdit打开J-link_V8.bin

1. 修改FF00h行

由原来的
```
76 B4 32 01 FF FF FF FF FF FF FF FF FF FF FF FF
```
变更为：
```
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF
```
![bits info](https://gitee.com/xiaobin80/csdn/raw/master/images/132134016452191.png)



2. 修改FF30h和FF40h行

由原来的

(1) FF30h
```
68 42 50 2C 4A 46 6C 61 73 68 2C 47 44 42 46 75
```
(2) FF40h
```
6C 6C 00 FF 80 20 20 20 7B E2 B7 DD C4 07 C2 0A
```


变更为：
![bits info](https://gitee.com/xiaobin80/csdn/raw/master/images/132134030824419.png)
红色即为修改的数值。

 
# init j-link
执行一遍《[修复山寨版的J-Link](https://xiaobin80.gitee.io/csdn/post/jlink-fixed1/)》。


# set sn
> 注意:
>> a. 安装完J-Link驱动后，便包含J-Link Commander程序。    
>> b. 20150813为自定义设置，但，必须是8位数字。

在”J-Link Commander”执行升级操作，并设置S/N。
```
J-Link>exec setsn=20150813
```
