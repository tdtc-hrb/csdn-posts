---
title: "TDTC Demo"
description: "c#, c++"
date: 2022-12-17T08:18:18+08:00
---

- 系统需求    
  Mixed(x86 and x64): Windows 10 - Windows Vista    
  Pure(x86): Windows XP - Windows 3.1    

  ref "VC Redist"

- 注册下列COM组件
```
xbfInfo.dll
repoInfo.dll
```

# callXBFLib
> 生成/读取xbf文件.

## 文档请参考

- [callXBFLibrary - 调用非托管DLL](https://tdtc-hrb.github.io/csdn/post/c2plus-unmanaged-dll/)
- [callXBFLibrary - 调用COM](https://tdtc-hrb.github.io/csdn/post/c2plus-com-dll/)

## 配置文件
> xbfConfig.ini

```bash
[config]
type = MySQL

[MS-Access]
DB = v:\fde.mdb
Pwd = wwsrts12

[MSSQL]
Server = ddrwaas2
DB     = dddrrewq
User   = ttrrre
Pwd    = qwerty

[MySQL]
Server = 192.168.1.107
DB     = mysql
User   = DBAdmin
Pwd    = xbfirst
```

# repoFmtGen
> 生成固定格式的报文。

### 文档请参考
- [读取SafeArray数据](https://tdtc-hrb.github.io/csdn/post/com-safearray-vc/)

## 运行效果
![program ui](https://github.com/tdtc-hrb/csdn/raw/master/images/20151225213801939.png)

## IE调用repoInfo.dll
> IE6/7/8/9/10/11

### 文档请参考
- [创建COM](https://tdtc-hrb.github.io/csdn/post/com-wizards-vc/)
- [使用IE测试COM](https://tdtc-hrb.github.io/csdn/post/com-ie-test/)
- [铁路车号读取 - IE](https://tdtc-hrb.github.io/csdn/post/com-ie-repoinfo/)
#### repoInfo_test.html
![ie ui](https://github.com/tdtc-hrb/csdn/raw/master/images/win81ie11-repinfo.png)


# testMySQL
> 使用MySQL数据库。

- [baidu-yun](https://pan.baidu.com/s/1J5fIUIn4IdBebITKfNGfdw) 提取码: ckwc

如果datagrid增加序号，增加如下代码：
```c sharp
DataColumn dc = new DataColumn();
dc.ColumnName = "序号";
dc.AutoIncrement = true;
dc.AutoIncrementSeed = 1;
dc.AutoIncrementStep = 1;
table.Columns.Add(dc);
```

## 文档请参考
- [MySQL Connector/Net 的简单使用](https://blog.csdn.net/xiaobin_HLJ80/article/details/21200619)

# testSQLite
  使用SQLite数据库.

## 文档请参考
- [SQLite在.net 的应用](https://github.com/tdtc-hrb/testSQLite)
- [SQLite在.net core的Image应用](https://bitbucket.org/li_guibin/imageapp)
- 组件    
  Use the Gmail account, published in bitbucket.org.
  
  
# [VC Redist](https://docs.microsoft.com/en-US/cpp/windows/latest-supported-vc-redist)
|version|x64|x86|
|-|-|-|
| vc 14.x | [vs2015/17/19/22](https://aka.ms/vs/17/release/vc_redist.x64.exe) | [vs2015/17/19/22](https://aka.ms/vs/17/release/vc_redist.x86.exe) |
| vc 12.0 for win7+ | [vs2013](https://aka.ms/highdpimfc2013x64enu) | [vs2013](https://aka.ms/highdpimfc2013x86enu) |
| vc 12.0 for Win XP] | [vs2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784) | [vs2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784) |
| vc 11.0(update 4) | [vs2012](https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe) | [vs2012](https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe) |
| vc 10.0(sp1) | [vs 2010](https://download.microsoft.com/download/1/6/5/165255E7-1014-4D0A-B094-B6A430A6BFFC/vcredist_x64.exe) | [vs 2010](https://download.microsoft.com/download/1/6/5/165255E7-1014-4D0A-B094-B6A430A6BFFC/vcredist_x86.exe) |
| vc 9.0(sp1) | [vs 2008](https://download.microsoft.com/download/5/D/8/5D8C65CB-C849-4025-8E95-C3966CAFD8AE/vcredist_x64.exe) | [vs 2008](https://download.microsoft.com/download/5/D/8/5D8C65CB-C849-4025-8E95-C3966CAFD8AE/vcredist_x86.exe) |

## Pure
> Windows Vista-
- vc 9.0
- vc 10.0
- vc 11.0    
Only deploy the program, can not install the VS2012 development environment.
- vc 12.0    
Only deploy the program, can not install the VS2013 development environment.

## Mixed
> Windows Vista+
- vc 14.x    
vs2015/17/19/22
