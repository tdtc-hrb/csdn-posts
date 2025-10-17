---
title: "使用物理网卡"
description: "VMware"
date: 2023-01-18T14:08:08+08:00
---

# VMware
- Host OS    
Win10(x64)

-Guest OS    
Ubuntu 14.04(x64)

目的：Ubuntu独立使用1块网卡与局域网中的设备或主机通讯。

问题：宿主机器有两个网卡

（1）无线网卡（2）有线网卡


## VMware Network
[overview](https://stackoverflow.com/a/54232899)
|          | VM <-> Host | VM1 <-> VM2 | VM -> Internet | VM <- Internet |
|----------|-------------|-------------|----------------|----------------|
| HostOnly | Yes         | Yes         | No             | No             |
| Internal | No          | Yes         | No             | No             |
| Bridged  | Yes         | Yes         | Yes            | Yes            |
| NAT      | No          | No          | Yes            | Port forward   |

### 编辑->虚拟网络编辑器
![vmware ui](https://gitee.com/xiaobin80/csdn/raw/master/images/20171227034323068.png)
默认设置

我们更改桥接模式中“桥接到”，即可。（由自动更改为下图）
![vmware ui](https://gitee.com/xiaobin80/csdn/raw/master/images/20171227034811154.png)

### 网络适配器
![vmware ui](https://gitee.com/xiaobin80/csdn/raw/master/images/20171227035131591.png)


# NFS
> AT91SAM9260 in terminal

启动NFS，工作正常。

输出信息如下：
```bash
This is the boot programm!鶷his is the boot programm!
Load UBOOT, please wait......
Load UBOOT from 0x10010000 to 0x23f00000 Over


U-Boot 1.3.4 (Mar 18 2015 - 07:00:42)

DRAM:  64 MB
MAN_ID: 0x00bf
PRO_ID: 0x234b
Flash:  2 MB
NAND:  NAND device: Manufacturer ID: 0xec, Chip ID: 0x76 (/狺掰掰?NAND 64
MiB 3,3V 8-bit)
64 MiB
DataFlash:AT45DB161
Nb pages:   4096
Page Size:    528
Size= 2162688 bytes
Logical address: 0xD0000000
Area 0: D0000000 to D0003FFF (RO)
Area 1: D0004000 to D0007FFF
Area 2: D0008000 to D0037FFF (RO)
Area 3: D0038000 to D020FFFF
In:    serial
Out:   serial
Err:   serial
PHY Detected (mdio-addr 1, ID 0x0013:0x78e2)
link up, 100Mbps, full-duplex
Hit any key to stop autoboot:  0
## Booting kernel from Legacy Image at 20400000 ...
   Image Name:   Linux-2.6.30
   Image Type:   ARM Linux Kernel Image (uncompressed)
   Data Size:    1439844 Bytes =  1.4 MB
   Load Address: 20008000
   Entry Point:  20008000
   Verifying Checksum ... OK
   Loading Kernel Image ... OK
OK

Starting kernel ...

Uncompressing Linux.............................................................
............................... done, booting the kernel.
Linux version 2.6.30 (root@ubuntu) (gcc version 4.3.2 (crosstool-NG 1.19.0) ) #1
 Fri Mar 27 07:55:15 PDT 2015
CPU: ARM926EJ-S [41069265] revision 5 (ARMv5TEJ), cr=00053177
CPU: VIVT data cache, VIVT instruction cache
Machine: Atmel AT91SAM9260-EK
Memory policy: ECC disabled, Data cache writeback
Clocks: CPU 199 MHz, master 99 MHz, main 18.432 MHz
Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 16256
Kernel command line: console=ttyS0,115200 ip=dhcp root=/dev/nfs rw
NR_IRQS:192
AT91: 96 gpio irqs in 3 banks
PID hash table entries: 256 (order: 8, 1024 bytes)
Console: colour dummy device 80x30
console [ttyS0] enabled
Dentry cache hash table entries: 8192 (order: 3, 32768 bytes)
Inode-cache hash table entries: 4096 (order: 2, 16384 bytes)
Memory: 64MB = 64MB total
Memory: 61956KB available (2484K code, 222K data, 120K init, 0K highmem)
Calibrating delay loop... 99.32 BogoMIPS (lpj=496640)
Mount-cache hash table entries: 512
CPU: Testing write buffer coherency: ok
net_namespace: 296 bytes
NET: Registered protocol family 16
AT91: Power Management
AT91: Starting after user reset
bio: create slab <bio-0> at 0
SCSI subsystem initialized
usbcore: registered new interface driver usbfs
usbcore: registered new interface driver hub
usbcore: registered new device driver usb
NET: Registered protocol family 2
IP route cache hash table entries: 1024 (order: 0, 4096 bytes)
TCP established hash table entries: 2048 (order: 2, 16384 bytes)
TCP bind hash table entries: 2048 (order: 1, 8192 bytes)
TCP: Hash tables configured (established 2048 bind 2048)
TCP reno registered
NET: Registered protocol family 1
NetWinder Floating Point Emulator V0.97 (double precision)
msgmni has been set to 121
alg: No test for stdrng (krng)
io scheduler noop registered
io scheduler anticipatory registered (default)
atmel_usart.0: ttyS0 at MMIO 0xfefff200 (irq = 1) is a ATMEL_SERIAL
atmel_usart.1: ttyS1 at MMIO 0xfffb0000 (irq = 6) is a ATMEL_SERIAL
atmel_usart.2: ttyS2 at MMIO 0xfffb4000 (irq = 7) is a ATMEL_SERIAL
brd: module loaded
loop: module loaded
ssc ssc.0: Atmel SSC device at 0xc4820000 (irq 14)
Driver 'sd' needs updating - please use bus_type methods
MACB_mii_bus: probed
eth0: Atmel MACB at 0xfffc4000 irq 21 (20:00:00:00:00:00)
eth0: attached PHY driver [LXT971] (mii_bus:phy_addr=ffffffff:01, irq=-1)
NAND device: Manufacturer ID: 0xec, Chip ID: 0x76 (Samsung NAND 64MiB 3,3V 8-bit
)
AT91 NAND: 8-bit, Software ECC
Scanning device for bad blocks
Creating 1 MTD partitions on "atmel_nand":
0x000000000000-0x000004000000 : "AT91 NAND my_P1"
ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
at91_ohci at91_ohci: AT91 OHCI
at91_ohci at91_ohci: new USB bus registered, assigned bus number 1
at91_ohci at91_ohci: irq 20, io mem 0x00500000
usb usb1: configuration #1 chosen from 1 choice
hub 1-0:1.0: USB hub found
hub 1-0:1.0: 2 ports detected
Initializing USB Mass Storage driver...
usbcore: registered new interface driver usb-storage
USB Mass Storage support registered.
udc: at91_udc version 3 May 2006
mice: PS/2 mouse device common for all mice
input: gpio-keys as /class/input/input0
rtc-at91sam9 at91_rtt.0: rtc core: registered at91_rtt as rtc0
IRQ 1/rtc0: IRQF_DISABLED is not guaranteed on shared IRQs
rtc-at91sam9 at91_rtt.0: rtc0: SET TIME!
Registered led device: ds5
Registered led device: ds1
TCP cubic registered
NET: Registered protocol family 17
RPC: Registered udp transport module.
RPC: Registered tcp transport module.
rtc-at91sam9 at91_rtt.0: hctosys: unable to read the hardware clock
Sending DHCP requests .<6>eth0: link up (100/Full)
, OK
IP-Config: Got DHCP answer from 192.168.0.135, my address is 192.168.0.251
IP-Config: Complete:
     device=eth0, addr=192.168.0.251, mask=255.255.255.0, gw=255.255.255.255,
     host=192.168.0.251, domain=example.org, nis-domain=(none),
     bootserver=192.168.0.135, rootserver=192.168.0.135, rootpath=/opt/tdtc/rfs
Looking up port of RPC 100003/2 on 192.168.0.135
Looking up port of RPC 100005/1 on 192.168.0.135
VFS: Mounted root (nfs filesystem) on device 0:12.
Freeing init memory: 120K
init started: BusyBox v1.23.2 (2015-11-09 22:59:50 EST)
Setting hotplug handler: [ OK ]
Creating device files: [ OK ]
Setting timezone and system clock: [OK]
Starting system logging.
Configuring network interfaces: ifdown: interface lo not configured
ip: RTNETLINK answers: File exists
failed
Generating RSA Key...
Will output 1024 bit rsa secret key to '/etc/dropbear/dropbear_rsa_host_key'
Generating key, this may take a while...
Public key portion is:
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgwCE5v/Ap3hCcJwDxLtDe3e4E6kQtgyBb1FKdFiJwmXa
FIgbaguUjNyl8uq4yNbU9ewsmyqTHGtKShvxpJAD789cL/vMfHbKNxdBhwJ+dpuvyYtd/wexNcZ3EMPJ
mvqqNXp0K/oTG0GdM1cnHES1AJQe26RYgiH2ew8DaATycT6IugBJ root@at91sam9260_ek
Fingerprint: md5 48:fb:27:a2:8f:fc:0f:68:6a:75:00:98:13:36:c7:28
Generating DSS Key...
Will output 1024 bit dss secret key to '/etc/dropbear/dropbear_dss_host_key'
Generating key, this may take a while...
Public key portion is:
ssh-dss AAAAB3NzaC1kc3MAAACBAP6C7qLaTYM4bGxHSekRnhqUV61kUFCAWv1LyW6fPbMlJlIjpv5B
Q1FRhsucQjY4BKfPEjLUB46veq0BMuTSs5ADVgrOKBXE5GnPu8oT04XZu+UjWO5wgYD6G1MyFVlzuY6m
N5i3jdYpNE3J4YaYfC0xdUNn8WWkJH7cdSaiB3OTAAAAFQCfUA2AFBa9DQ+sUFOlAN3KvvaSywAAAIAs
SOR8U0D+kO3aGWr+R73zH/oR2wqF/2TuRUqxIZQo6tzfuhmqRlvEtQ+2cN+NOac8BWO7PEa4txVI8aKe
GC9AODlknbdorlYWfxqbqD4H05JQlCr/FY43KctjSSgcg81Ezk3Rh50CbkLk12E7EUh7e0khohuWOZhA
AeH9v0xFrQAAAIAucDdk5oGeAKQ67pmgUake69h4oRUCN727lZFjRrGhduTfSYo1IyLZ78/CGXab8sF4
aTSGa2PimN8evxyz2zAxghNqRpw1FVnjuglNjPH4GbbAPcRdVIYut19nFG7J8ULDvIDkIaHQHbdN1M3d
/803WER1YCaJhWxq5BvRYgf10g== root@at91sam9260_ek
Fingerprint: md5 b4:f5:28:b4:c4:f2:27:15:32:f6:77:56:44:f5:da:85
Starting dropbear sshd: OK


BusyBox v1.23.2 (2015-11-09 22:59:50 EST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

# ls
0        dev      lib      mnt      root     share    usr
bin      etc      linuxrc  opt      run      sys      var
config   home     media    proc     sbin     tmp
#
```
