---
title: "U-boot V1.3.4"
description: "U-boot V1.3.4移植到at91sam9260ek"
date: 2020-05-16T01:58:08+08:00
---

关于移植在U-boot文档中这样介绍：
README
```txt
If the system board that you have is not listed, then you will need
to port U-Boot to your hardware platform. To do this, follow these
steps:

1.  Add a new configuration option for your board to the toplevel
    "Makefile" and to the "MAKEALL" script, using the existing
    entries as examples. Note that here and at many other places
    boards and other names are listed in alphabetical sort order. Please
    keep this order.
2.  Create a new directory to hold your board specific code. Add any
    files you need. In your board directory, you will need at least
    the "Makefile", a "<board>.c", "flash.c" and "u-boot.lds".
3.  Create a new configuration file "include/configs/<board>.h" for
    your board
3.  If you're porting U-Boot to a new CPU, then also create a new
    directory to hold your CPU specific code. Add any files you need.
4.  Run "make <board>_config" with your new name.
5.  Type "make", and you should get a working "u-boot.srec" file
    to be installed on your target system.
6.  Debug and solve any problems that might arise.
    [Of course, this last step is much harder than it sounds.]
```

# 0. 准备工作
## 1) 下载
[V1.3.4](ftp://ftp.denx.de/pub/u-boot/u-boot-1.3.4.tar.bz2)

[补丁](ftp://www.at91.com/pub/uboot/u-boot-1.3.4-exp.5/u-boot-1.3.4-exp.5.diff)

## 2) 安装
u-boot安装此处省略

### 补丁安装
拷贝u-boot-1.3.4-exp.5.diff到u-boot-1.3.4
```bash
cat u-boot-1.3.4-exp.5.diff |patch -p1
```

# 1. 添加开发板
## 1) MAKEALL
```mak
#########################################################################

## ARM9 Systems

#########################################################################

LIST_ARM9="                    \
  at91sam9260ek      at91sam9261ek      at91sam9263ek                 \
```

## 2) Makefile
```mak
#########################################################################

## ARM926EJS Systems

#########################################################################



at91sam9260ek_config : unconfig

  @$(MKCONFIG)$(@:_config=) arm arm926ejs at91sam9260ek atmel at91sam926x
```

- arm:           CPU架构
- arm926ejs:     CPU型号
- at91sam9260ek: 开发板名称
- atmel:         所属厂商
- at91sam926x:   SoC

# 2. 文件构成
## 架构
  arch-at91sam926x文件夹(u-boot-x.x.x/../include/asm/)

## CPU
at91sam926x文件夹(u-boot-x.x.x/../cpu/arm926ejs/)

### 删除LCD和USB支持
```txt
lcd.c、lcd_lut.h、usb_ohci.c、usb_ohci.h
```

### 删除DM9000物理网卡支持
ether.c
```txt
Ln25: #ifndef CONFIG_DRIVER_DM9000 /*SAM9261EK uses DM9000 Phy */
Ln30: #include <dm9161.h>
Ln457: #endif  /*CONFIG_DRIVER_DM9000 */
```

## 开发板
at91sam9260ek文件夹(u-boot-x.x.x/../board/)

### 删除DM9000物理网卡
dm9161a.c

## 开发板配置
> at91sam9260ek.h(u-boot-x.x.x/../include/configs/)

```c
#include<cmd_confdefs.h>
```
变更为：
```c
#include< config_cmd_default.h>
```

## Ethernet部分
拷贝at91_net.h到：u-boot-x.x.x/include/

各文件介绍：
### MII物理寄存器定义:
  ../include/miipyh.h

### 函数头文件:
  ../include/at91_net.h

### 函数实现:
  ../cpu/arm926ejs/at91sam926x/ether.c

## flash部分
  覆盖dataflash.h到：u-boot-1.3.4/include/    
  覆盖dataflash.c到：u-boot-1.3.4/drivers/mtd/

## nand部分
### 1) nand文件夹
  拷贝位置：../drivers/mtd/nand    
并修改函数名：    
    DEBUG 变更为： MTDDEBUG

### 2) cmd_nand.c
  拷贝至：../common/

  并注释掉：
```c
//print_image_hdr(hdr);
```

## 中断部分
  删除../cpu/arm926ejs/下的interrupts.c，否则会出现多重定义的问题！

# 3. 宏修改
```c
 CFG_*_* --〉 CONFIG_*_*
```

## 1) net
```c
   #if(CONFIG_COMMANDS & CFG_CMD_NET)
```
   --〉
```c
  #if defined(CONFIG_CMD_NET)
```

## 2) nand
```c
  #if(CONFIG_COMMANDS & CFG_CMD_NAND)
```
  --〉
```c
  #ifdefined(CONFIG _CMD_NAND)
```

### Nand命令
  config_cmd_default.h：添加宏定义CONFIG_CMD_NAND；

  宏定义来源：config_cmd_all.h


# resource
  适合特定AT91sam9260EK开发板的[U-boot V1.3.4](http://pan.baidu.com/s/1dEnbzt3)
