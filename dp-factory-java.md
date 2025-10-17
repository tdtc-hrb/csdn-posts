---
title: "工厂模式"
description: "java实现"
date: 2020-05-08T10:08:08+08:00
---
09年5月CSDN一网友提出如下问题：

设计一个用于管理银行客户的类BankCustomer：
	仅描述客户的几个重要方面：
		帐号、身份证号、姓名、联系方式、密码、账户余额。
	所有的成员变量均用private访问控制，因此每一个成员变量就要有相应的存取器
	（getter和setter，即获取和设置其值的相应的成员方法。需要setter还是getter，还是两者都要，视情况而定）

	成员方法：
	开户（开户时必须要有身份证号），系统自动生成帐号，帐号使用系统时间（格式："yyyyMMddHHmmss"14位），初始密码为“666666”。
	注意开户和构造方法之间的关系。
	存钱、取钱、显示账户信息、修改密码（密码最短要六位）
	怎样在main中使用这个类，自行安排，要表现出你设计的类的各个方面，并在main中用英语加以注释

类图如下：（使用Enterprise Architect绘制）    
![ea ui](https://gitee.com/xiaobin80/csdn/raw/master/images/20130711014414609.png)

![ea ui](https://gitee.com/xiaobin80/csdn/raw/master/images/20130707190101296.png)

------------factory部分-------------

  Customer：        抽象类（factory祖先类）

  BankCustomer：继承类（factory类）

 
  〉〉〉〉〉〉〉〉〉〉扩展部分

  ContactWay：联系方式（factory引用类）

  IM        ：实时消息（ContactWay引用类）

------------product部分-------------  

  Bank：       接口（product接口）

  Account：    实现类（concrete product类）
