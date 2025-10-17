---
title: "关于Spring Security 4例子的补充"
description: "数据表, pojo and 依赖库"
date: 2020-04-10T11:26:39+08:00
---
[Spring Security 4 Hibernate Password Encoder Bcrypt Example](http://websystique.com/spring-security/spring-security-4-password-encoder-bcrypt-example-with-hibernate)

# 1. 数据表
资料来源于[spring-security-remember-me-example](https://www.mkyong.com/spring-security/spring-security-remember-me-example/)
```sql
CREATE TABLE `persistent_logins` (
      `series` varchar(64) NOT NULL,
      `username` varchar(64) NOT NULL,
      `token` varchar(64) NOT NULL,
      `last_used` datetime NOT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


    ALTER TABLE `persistent_logins`
      ADD PRIMARY KEY (`series`);
    COMMIT;
```
# 2. pojo
> User.java   

其中的缺省值：
```sql
@Column(name = "state", nullable=true)
private String state = "Inactive";
```

# 3. 依赖库
> pom.xml

- springframework.version    
v4.3.14.RELEASE
- springsecurity.version     
v4.2.4.RELEASE
- hibernate.version    
v5.1.13.Final
- mysql.connector.version    
v5.1.46
- validation api    
V2.0.1
- hibernate validator    
v5.4.2
- Logback    
V1.2.3
- maven compiler plugin    
V3.7.0
- maven war plugin    
V3.2.0
