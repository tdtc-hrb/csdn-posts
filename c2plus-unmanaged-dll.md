---
title: "调用非托管DLL"
description: "c#"
date: 2023-01-03T00:08:08+08:00
---
> IDE: vs2008 Express(sp1)

callXBFLibrary
===

我们需要使用API函数来读取INI文件。

# 建立P/Invoke应用类
![vs2008 ui1](https://github.com/tdtc-hrb/csdn/raw/master/images/20200514014607.png)

![vs2008 ui2](https://github.com/tdtc-hrb/csdn/raw/master/images/20200514014830.png)

在[pinvoke.net](http://pinvoke.net/index.aspx)中查找Win32 API在C#定义。
![vs2008 ui](https://github.com/tdtc-hrb/csdn/raw/master/images/20140303141518625.png)

## GetPrivateProfileString
读取Ini文件的API
```c sharp
[DllImport("kernel32.dll", CharSet=CharSet.Unicode)]
static extern uint GetPrivateProfileString(
   string lpAppName,
   string lpKeyName,
   string lpDefault,
   StringBuilder lpReturnedString,
   uint nSize,
   string lpFileName);
```
> 注意：DllImport属性来自using System.Runtime.InteropServices;

- lpAppName    
  小节名
- lpKeyName    
  键名
- lpDefault    
  缺省值
- lpReturnedString     
  返回值
- nSize     
  返回值长度
- lpFileName    
  读取文件的名称

### import class
```c sharp
  using System.Runtime.InteropServices;
```

## readIniFileVal
建立一个公共方法，以供程序使用。
```c sharp
public string readIniFileVal(string iniFileName, string section, string key)
{
    StringBuilder retStrBuilder = new StringBuilder(256);
    GetPrivateProfileString(
        section,
        key,
        "",
        retStrBuilder,
        256,
        iniFileName);
    return retStrBuilder.ToString();
}
```

# code
> Cwin32API.cs

```c sharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Runtime.InteropServices;

namespace callXBFLibrary
{
    class Cwin32API
    {
        [DllImport("kernel32.dll", CharSet = CharSet.Unicode)]
        private static extern uint GetPrivateProfileString(
           string lpAppName,
           string lpKeyName,
           string lpDefault,
           StringBuilder lpReturnedString,
           uint nSize,
           string lpFileName);

        private string iniFileName;


        public Cwin32API(string fileName)
        {
            this.iniFileName = fileName;
        }

        /**
         * <p> Read Ini File Value </p>
         *
         * @param name="section"
         * @param name="key"
         * @returns string
         *
         **/
        public string readIniFileVal(string section, string key)
        {
            StringBuilder retStrBuilder = new StringBuilder(256);
            GetPrivateProfileString(
                section,
                key,
                "",
                retStrBuilder,
                256,
                iniFileName);
            return retStrBuilder.ToString();
        }
    }

}
```
