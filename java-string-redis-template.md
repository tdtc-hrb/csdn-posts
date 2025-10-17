---
title: "StringRedisTemplate - redis-cli"
description: "使用StringRedisTemplate读取redis-cli输入的数据"
date: 2020-04-10T13:26:39+08:00
---

# 1. autowired
```java
@Autowired
StringRedisTemplate stringRedisTemplate;
```

# 2. get/set
```java
public void redisCliTest() {
      String cliVal = stringRedisTemplate.opsForValue().get("mykey2");
      System.out.println("redis-cli value: " + cliVal);
}
```

# code
> UserController.java

```java
package com.example.demo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.example.demo.model.UserDetails;
import com.example.demo.service.UserDetailsService;

@Controller
@RequestMapping("/user")
public class UserController {

	@Autowired
	private UserDetailsService userService;

	@Autowired
	StringRedisTemplate stringRedisTemplate;

	@RequestMapping(value = "/listPage", method = RequestMethod.GET)
	public String findAll(Model model) {
		List<UserDetails> users = userService.getUsers();

		model.addAttribute("users", users);

		for(UserDetails user : users) {
			System.out.println(user.getEmail() + " " + user.getName());
		}

		redisCliTest();

		return "userList";
	}

	public void redisCliTest() {
        String cliVal = stringRedisTemplate.opsForValue().get("mykey2");
        System.out.println("redis-cli value: " + cliVal);
	}
}
```

## run
http://localhost:8080/user/listPage    
![browser info](https://gitee.com/xiaobin80/csdn/raw/master/images/20180921092240670.png)

## redis-cli

![cmd info](https://gitee.com/xiaobin80/csdn/raw/master/images/20180921092553827.png)


## console info
> spring boot(v2.0.5)

```bash
2018-09-21 09:17:38.284  INFO 9992 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-09-21 09:17:38.284  INFO 9992 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization started
2018-09-21 09:17:38.295  INFO 9992 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : FrameworkServlet 'dispatcherServlet': initialization completed in 10 ms
2018-09-21 09:17:38.317  INFO 9992 --- [nio-8080-exec-1] o.h.h.i.QueryTranslatorFactoryInitiator  : HHH000397: Using ASTQueryTranslatorFactory
2018-09-21 09:17:38.327 DEBUG 9992 --- [nio-8080-exec-1] org.hibernate.SQL                        :
    select
        userdetail0_.id as id1_0_,
        userdetail0_.address as address2_0_,
        userdetail0_.email as email3_0_,
        userdetail0_.name as name4_0_,
        userdetail0_.password as password5_0_,
        userdetail0_.username as username6_0_
    from
        user_details userdetail0_
    order by
        userdetail0_.id DESC
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.333  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.334  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
2018-09-21 09:17:38.334  INFO 9992 --- [nio-8080-exec-1] com.example.demo.dao.UserDetailsImpl     : test
test4@163.com test
test4@163.com test
test4@163.com test
test3@163.com test
test2@163.com test
test1@163.com test
test4@163.com test
test4@163.com test
test4@163.com test
redis-cli value: this is test message2.
```

# Reference
- [Using RedisTemplate set a value but get Nil from Terminal Redis-CLI](https://stackoverflow.com/questions/34736265/using-redistemplate-set-a-value-but-get-nil-from-terminal-redis-cli)
