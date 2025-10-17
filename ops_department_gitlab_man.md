---
title: "GitLab使用"
description: "私有域"
date: 2020-05-15T04:08:08+08:00
---

# 一、注册用户
> Name：姓名
> Username：用户名
> 密码：8位

![gitlab ui1](https://gitee.com/xiaobin80/csdn/raw/master/images/20170911111349597.png)![gitlab ui2](https://gitee.com/xiaobin80/csdn/raw/master/images/20170911111359202.png)

> 等待管理员解锁。

# 二、增加SSH
## 1. 生成SSH
在“开始”菜单，选择“TortoiseGit”中的“PuttyGen”
![TortoiseGit ui1](https://gitee.com/xiaobin80/csdn/raw/master/images/20130903013014140.png)


点击“Generate”按钮，然后，鼠标在图中红色方框的区域内不停的移动，即可生成密钥。    
“Public key for pasting into OpenShh authorized_keys file:”所有内容既是我们需要的。
![TortoiseGit ui2](https://gitee.com/xiaobin80/csdn/raw/master/images/20130903013211546.png)


## 2. 设置SSH
用户登录后，进入“ProfileSettings”->“SSH Keys”

![gitlab ui3](https://gitee.com/xiaobin80/csdn/raw/master/images/20170911111248125.png)![gitlab ui4](https://gitee.com/xiaobin80/csdn/raw/master/images/20170911111312903.png)

点击“Add SSH Key”
![gitlab ui5](https://gitee.com/xiaobin80/csdn/raw/master/images/20170911111422389.png)

把上面生成的ssh复制到key中（title会自动生成），点击“Add key”
![gitlab ui6](https://gitee.com/xiaobin80/csdn/raw/master/images/20170911111441603.png)
