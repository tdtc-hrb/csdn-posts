---
title: "UML工具Rose"
date: 2020-05-17T10:08:08+08:00
---

# 安装Rose2007

下载并安装
```bt
ed2k://|file|%5BIBM%E8%BD%AF%E4%BB%B6%E7%B3%BB%E5%88%97%5D.IBM.Rational.Rose.Enterprise.v7.0-TFTISO.bin|621038544|71d09610bb53d2d905d278675b333c75|h=utnlhfjwnzjdp2wecfeujoptd7pqlrps|/
```

默认安装即可。

# lic
序号有两种方式：本机和导入Lic

## 本机
本地机器作为lic服务器

### 1. 修改授权文件。    
    打开license.dat文件，    
    大约在文件的中间位置有：SERVER Microsoft ANY DAEMON rational “C:\Program Files\Rational\common\rational.exe”    
    将其修改为：SERVER 计算机名　ANY DAEMON rational “自己安装的目录\rational.exe”后，保存

### 2. 拷贝授权检查程序。    
    将license.dat、 lmgrd.exe 、rational.exe三个文件拷贝到：安装目录\rational\common\

### 3. 将授权程序放到控制面板。
    将flexlm.cpl拷贝到system32目录下。如win2000系统中为C:\WINNT\system32目录。

### 4. 设置授权程序。    
    打开FLEXlm License Manager（控制面板中），    
    在Setup页中lmgrd.exe右侧目录填写：C:\Program Files\Rational\Common\lmgrd.exe（若为默认安装目录）    
    License File右侧目录写为：C:\Program Files\Rational\Common\license.dat

### 5. 查看状态。    
    回到Control页，
    点击Start，若出现”Server Started”，则表示已经成功，    
    可以点击Status,若状态为：计算机名：license server UP(MASTER)则成功（否则，Start->Status来回点~~）。

### 6. 修改Server Name。    
    打开Rational License Key Administrator，     
    点击工具栏中的"Start WIzard", 点击下一步，    
    选择"Point to a Rational License Server to get my licenses" , 点击下一步，    
    在Server Name中的名字改为自己的计算机名即可。

## 导入License
1. IBM Rational License Key Administrator
2. Import a Rational License File
- Browse
```bash
../crack/license.udp
```

# 业务用例
![busines use-case](https://gitee.com/xiaobin80/csdn/raw/master/images/20140427132252406.png)
