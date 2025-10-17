---
title: "修复山寨版的J-Link"
description: "J-Link两篇之一"
date: 2020-05-05T08:08:08+08:00
---

Fixed J-Link
====

# Erase
1. Power On
2. Jump "ERASE"(JP3)
3. Wait for 5s
4. Break "ERASE"(JP3)
5. Power Off

# TST
1. Jump "TST"(JP1)
2. Power On
3. Wait for 10s
4. Power Off
5. Break "TST"(JP1)

重新上电后，电脑会弹出发现新硬件：“atm6124.Sys ATMEL AT91xxxxx Test Board”。

Then PC will find the "ATMEL AT91xxxxx Test Board", and Led on("ERR")

# SAM-PROG    
> 注意：必须在x86（32位）下运行！
## Down      
[at91 isp V1.12](http://www.atmel.com/images/Install_AT91-ISP_v1.12.exe)

## Exec
> SAM-PROG v2.4

### Setup
first, click "Browse" choose "J-link_V8.bin"    

Second, Select "Set Security Bit"


> Note: J-link_V8.bin is example file, It must be modified to use.

### Write
First, Give this USB device Power On

Second, Click "Wite Flash" button

  When burning is complete "Status" box in the "In progress" is 0 and "Active Connection" is 1.
    Led on("RUN") will light.

---
(Chineses)          
（1）首先找到PCB板子的Erase脚和TST脚

（2）这两个重要的管脚找到之后，然后通过USB数据线连接J-Link和电脑，
    给J-Link供电（注意这一步小灯可能不亮，但电源已经加到J-Link板子上了）； 

（3）短接Erase区的两个过孔（即Erase与VDD3.3v）约5s以后，断开该连接，这时擦除完毕，
    最后断开USB电源，停止给J-Link供电。（注意先后顺序）

（4）短接TST区的两个过孔（即TST与VDD3.3v），然后再连接USB数据线给J-Link供电（注意顺序），约10s以后，
    拔掉USB电源，再断开TST区的连接，这时进入编程模式；


下载地址：http://pan.baidu.com/s/1sj9Q7b3
