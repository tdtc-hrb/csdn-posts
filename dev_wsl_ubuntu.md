---
title: "WSL部署local服务"
description: "Ubuntu/Debian"
date: 2026-10-24T15:08:08+08:00
---
- Windows 10 21h2+
> 在powerShell的管理员模式下运行命令!

**注意：现在大部分的IDE（CLion, VS等）都使用WSL2!**

# 安装/卸载
ON:
```
Control Panel -> Uninstall a program -> Turn Windows features on or off, 
勾选“Windows Subsystem for Linux”
```
缺省为WSL1:
```ps
wsl -l -v
```

### WSL2
> If using WSL1, skip this step.

- Enable Virtual Machine feature
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
Reboot your Windows 10 host

- [WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
```powershell
wsl --update
```
- Set WSL 2 as your default version
```powershell
wsl --set-default-version 2
```

## Ubuntu
- [Ubuntu 2004](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions)
```powershell
Add-AppxPackage .\Ubuntu2404-240425.AppxBundle
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
- wsl2
- wsl1

## WSL2
- proxy

### proxy
在当前的WSL2 中设置代理很麻烦。

即使设置了 `networkingMode=Mirrored` 也无法工作！

## WSL1
- 访问WSL1服务
> WSL1与windows 10共有端口，所以Windows 10已使用的端口，不要再分配给WSL

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
