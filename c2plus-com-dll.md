---
title: "c#调用COM"
description: "callXBFLibrary"
date: 2025-01-19T18:08:08+08:00
---
!!! 除了IE的ActiveX项目，不推荐ATL !!!    
!!! ATL is not recommended except for IE ActiveX projects. !!!

## Add Reference
![vs2008 ui1](https://github.com/tdtc-hrb/csdn/raw/master/images/20200514021327.png)

![vs2008 ui2](https://github.com/tdtc-hrb/csdn/raw/master/images/20200514021444.png)

COM选项卡
![vs2008 ui3](https://github.com/tdtc-hrb/csdn/raw/master/images/20200514021640.png)
添加完成时

# 添加事件
- import class
```c sharp
using System.IO;
using xbfInfoLib;
```

## 写xbf文件
- 读Ini文件配置
```c sharp
Boolean flagMSSQL = false;
Boolean flagSecu = false;
string sectionName = ini.readIniFileVal("config", "type");
string strSrv = ini.readIniFileVal(sectionName, "Server");
if (!Equals(strSrv, ""))
    flagMSSQL = true;
if (flagMSSQL)
{
    strConn = "Provider=SQLOLEDB.1;"
        + "Password=" + ini.readIniFileVal(sectionName, "Pwd") + ";"
        + "Persist Security Info=True;"
        + "User ID=" + ini.readIniFileVal(sectionName, "User") + ";"
        + "Initial Catalog=" + ini.readIniFileVal(sectionName, "DB") + ";"
        + "Data Source=" + ini.readIniFileVal(sectionName, "Server");

}
else // MS Access
{
    string strPWD = ini.readIniFileVal(sectionName, "Pwd");
    if (!Equals(strPWD, ""))
        flagSecu = true;
    if (flagSecu)
    {
        strConn = "Provider=Microsoft.Jet.OLEDB.4.0;"
            + "Data Source=" + ini.readIniFileVal(sectionName, "DB") + ";"
            + "Persist Security Info=False;"
            + "Jet OLEDB:Database Password=" + ini.readIniFileVal(sectionName, "Pwd");
    }
    else
    {
        strConn = "Provider=Microsoft.Jet.OLEDB.4.0;"
            + "Data Source=" + ini.readIniFileVal(sectionName, "DB") + ";"
            + "Persist Security Info=True";
    }
}
```

- 调用COM接口
```c sharp
xbfInfoLib.FormatClass xbf = new FormatClass();
xbf.setFileInfo(strConn);
```

## 读xbf文件
```c sharp
xbfInfoLib.FormatClass xbf = new FormatClass();

string filePath = currDir + "/" + edtFileName.Text;
string strRet = xbf.getFileInfo(filePath);
MessageBox.Show(strRet);
```

# code
> Form1.cs

```c sharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.IO;
using xbfInfoLib;

namespace callXBFLibrary
{
    public partial class frmCall_XBF : Form
    {
        private Cwin32API ini;
        private string strConn;
        private string currDir;

        public frmCall_XBF()
        {
            InitializeComponent();

            currDir = Directory.GetCurrentDirectory();

            ini = new Cwin32API(currDir + "/xbfConfig.ini");
        }

        private void btnWriteXBF_Click(object sender, EventArgs e)
        {
            Boolean flagSQL = false;
            Boolean flagMSSQL = false;
            Boolean flagSecu = false;
            string sectionName = ini.readIniFileVal("config", "type");
            string strSrv = ini.readIniFileVal(sectionName, "Server");
            if (!Equals(strSrv, ""))
                flagSQL = true;
            if (flagSQL)
            {
                if (Equals(sectionName, "MSSQL"))
                    flagMSSQL = true;

                if (flagMSSQL)
                    strConn =
                        "dbType=0;"
                        + "Password=" + ini.readIniFileVal(sectionName, "Pwd") + ";"
                        + "Persist Security Info=True;"
                        + "User ID=" + ini.readIniFileVal(sectionName, "User") + ";"
                        + "Initial Catalog=" + ini.readIniFileVal(sectionName, "DB") + ";"
                        + "Data Source=" + ini.readIniFileVal(sectionName, "Server");
                else
                    strConn = "dbType=2;"
                        + "Data Source=" + ini.readIniFileVal(sectionName, "Server") + ";"
                        + "User Id=" + ini.readIniFileVal(sectionName, "User") + ";"
                        + "Password=" + ini.readIniFileVal(sectionName, "Pwd") + ";"
                        + "Database=" + ini.readIniFileVal(sectionName, "DB") + ";"
                        + "Convert Zero Datetime=True";

            }
            else // MS Access
            {
                string strPWD = ini.readIniFileVal(sectionName, "Pwd");
                if (!Equals(strPWD, ""))
                    flagSecu = true;
                if (flagSecu)
                {
                    strConn =
                        "dbType=1;"
                        + "Data Source=" + ini.readIniFileVal(sectionName, "DB") + ";"
                        + "Persist Security Info=False;"
                        + "Jet OLEDB:Database Password=" + ini.readIniFileVal(sectionName, "Pwd");
                }
                else
                {
                    strConn =
                         "dbType=1;"
                        + "Data Source=" + ini.readIniFileVal(sectionName, "DB") + ";"
                        + "Persist Security Info=True";
                }
            }

            MessageBox.Show(strConn);


            xbfInfoLib.FormatClass xbf = new FormatClass();
            xbf.setFileInfo(strConn);
        }

        private void btnReadXBF_Click(object sender, EventArgs e)
        {
            xbfInfoLib.FormatClass xbf = new FormatClass();

            string filePath = currDir + "/" + edtFileName.Text;
            string strRet = xbf.getFileInfo(filePath);
            MessageBox.Show(strRet);
        }
    }
}
````

# 部署WinForms
> 只保留x86/x64两种编译平台。

![vs2008 ui4](https://github.com/tdtc-hrb/csdn/raw/master/images/20200514033325.png)
