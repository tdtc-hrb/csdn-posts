---
title: "WSL部署local服务"
description: "Ubuntu/Debian"
date: 2024-12-12T18:08:08+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

- Windows 10 1809+    
缺省为WSL1
```ps
wsl -l -v
```

> 在powerShell的管理员模式下运行命令!

# 安装/卸载
ON:
```
Control Panel -> Uninstall a program -> Turn Windows features on or off, 
勾选“Windows Subsystem for Linux”
```

## Ubuntu
- [Ubuntu 2004](https://aka.ms/wslubuntu2004)

  从AppxBundle提取appx.
```powershell
Add-AppxPackage .\Ubuntu_2004.xxx.appx
```

### 卸载
如果“普通”方式不工作，请使用“命令行”。

- 普通
  右击“Ubuntu xxx” 图标，选择“Uninstall”即可。

- [命令行](https://docs.microsoft.com/zh-cn/windows/wsl/reference)
```bash
wslconfig /u
```

## Debian
- [down appx - debian v11](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions)
```powershell
Add-AppxPackage .\DistroLauncher-Appx_1.12.2.0_x64.appx
```

### WSL2
- Enable Virtual Machine feature
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
Reboot your Windows 10 host

-  WSL Linux kernel update package
```powershell
wsl --update
```
- Install
Click the "Debian" icon to install Debian.
 This takes some time.

Set a username and password for your Debian GNU/Linux installation when prompted

You are now logged in to your Debian system in command line mode

### exec
enter wsl:
```powershell
wsl --shutdown
wsl -u root
```

#### [sources list](https://mirrors.163.com/.help/debian.html)
- network
[temporary failure resolving](https://serverfault.com/a/1131182)
```powershell
echo "nameserver 8.8.8.8
nameserver 8.8.4.4" | sudo tee /etc/resolv.conf > /dev/null
```
- update
```powershell
echo "deb http://mirrors.163.com/debian/ bookworm main non-free contrib
deb http://mirrors.163.com/debian/ bookworm-updates main non-free contrib
deb-src http://mirrors.163.com/debian/ bookworm main non-free contrib
deb-src http://mirrors.163.com/debian/ bookworm-updates main non-free contrib
deb http://mirrors.163.com/debian-security/ bookworm-security main non-free contrib
deb-src http://mirrors.163.com/debian-security/ bookworm-security main non-free contrib" > /etc/apt/sources.list && \
apt update -y
```

#### usrmerge
- install
```powershell
apt install usrmerge
```
##### umount lib
If the following error message is displayed, it means v1 is mounting "/lib/modules".
```
FATAL ERROR:

/lib/modules/ is a mount point.
Probably this system is using User Mode Linux or some variant of Xen.

To continue the conversion please unmount /lib/modules/ (try the command
'umount -l /lib/modules/') and then try again.
Do not forget to reboot after the conversion is complete to have it
mounted again.
```
umount it:
```powershell
umount -l /lib/modules
```

#### Upgrade Debian 11 to 12
```powershell
apt upgrade -y
apt dist-upgrade -y
apt autoremove -y
```


# 使用
更改源列表:
```bash
sudo vi /etc/apt/sources.list
```
```
http://archive.ubuntu.com -> https://mirrors.aliyun.com/ubuntu/
```

## 文件互访
  首先，使用windows记事本，建立一个testTxtFile.txt；    
  然后，在WSL查看文件内容。
```bash
cd /mnt/c/Users/tdtc/
vi testTxtFile.txt
```
  最后，用WSL重命名文件名:
```bash
mv testTxtFile.txt testFile.txt
```
在windows就可以看到重命名的testFile.txt。


## 访问WSL服务
> WSL与windows 10共有端口，所以Windows 10已使用的端口，不要再分配给WSL

首先，在wsl中启动nginx；
```bash
sudo service nginx start
```

然后访问(回环地址)：127.0.0.1:90
![nginx info](https://gitee.com/xiaobin80/csdn/raw/master/images/20171216052918750.png)


# Ref
- [How to Configure Static IP Address on Ubuntu 20.04](https://linuxize.com/post/how-to-configure-static-ip-address-on-ubuntu-20-04/)
- [Installing Debian On Microsoft Windows Subsystem For Linux](https://wiki.debian.org/InstallingDebianOn/Microsoft/Windows/SubsystemForLinux)
- [Upgrade Debian 9 (current WSL) to Debian 12 (bookworm testing)](https://gist.github.com/bramtechs/50d724a33d37278d7ca003c6119c8fea)
