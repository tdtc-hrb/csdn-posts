---
title: "thymeleaf + Spring Boot + JPA"
date: 2022-05-10T10:26:39+08:00
---

# 1. 自动构建

## 1) [boot向导](https://start.spring.io/)
![project wizzard](https://github.com/tdtc-hrb/csdn/raw/master/images/springboot-dep.png)


## 2) pom依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.2-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>demo-jpa</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.2.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>
```

# 2. DML

## 1) DB

### application.properties
```ini
spring.datasource.driverClassName = com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=DBAdmin
spring.datasource.password=xbfirst
spring.datasource.tomcat.max-wait=20000
spring.datasource.tomcat.max-active=50
spring.datasource.tomcat.max-idle=20
spring.datasource.tomcat.min-idle=15

spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQLDialect
spring.jpa.properties.hibernate.id.new_generator_mappings = false
spring.jpa.properties.hibernate.format_sql = true

logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
```

### SQL
> table: user_details    

```sql
-- phpMyAdmin SQL Dump
-- version 4.8.0
-- https://www.phpmyadmin.net/
--
-- Host: localhost
-- Generation Time: Apr 22, 2018 at 02:15 AM
-- Server version: 5.5.59-MariaDB
-- PHP Version: 5.6.35

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET AUTOCOMMIT = 0;
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `test`
--

-- --------------------------------------------------------

--
-- Table structure for table `user_details`
--

CREATE TABLE `user_details` (
  `id` int(11) NOT NULL,
  `username` varchar(30) NOT NULL,
  `name` varchar(30) NOT NULL,
  `email` varchar(30) NOT NULL,
  `password` varchar(64) NOT NULL,
  `address` varchar(256) CHARACTER SET utf8 NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

--
-- Dumping data for table `user_details`
--

INSERT INTO `user_details` (`id`, `username`, `name`, `email`, `password`, `address`) VALUES
(1, 'test1', 'test', 'test1@163.com', '$2a$04$HcffZJMrgN0.lmEFpC4KTeeQhitLYzrH0uTCpGRJRBw4o03ms.qWa', 'wuhan-wuchang'),
(2, 'test2', 'test', 'test2@163.com', '$2a$04$HcffZJMrgN0.lmEFpC4KTeeQhitLYzrH0uTCpGRJRBw4o03ms.qWa', 'wuhan-hankou'),
(3, 'test3', 'test', 'test3@163.com', '$2a$04$HcffZJMrgN0.lmEFpC4KTeeQhitLYzrH0uTCpGRJRBw4o03ms.qWa', 'wuhan-hanyang');

--
-- Indexes for dumped tables
--

--
-- Indexes for table `user_details`
--
ALTER TABLE `user_details`
  ADD PRIMARY KEY (`id`);
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
```

## 2) POJO
> UserDetails.java

```java
package com.example.demo.model;

import java.io.Serializable;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "user_details")
public class UserDetails implements Serializable {

	private static final long serialVersionUID = 1593950786773788099L;

	@Id
	@GeneratedValue(strategy=GenerationType.AUTO)
	@Column(name = "id")
	private int userId;

	@Column(name = "username")
	private String userName;

	@Column(name = "name")
	private String name;

	@Column(name = "password")
	private String passwrod;

	@Column(name = "email")
	private String email;

	@Column(name = "address")
	private String address;

	public int getUserId() {
		return userId;
	}

	public void setUserId(int userId) {
		this.userId = userId;
	}

	public String getUserName() {
		return userName;
	}

	public void setUserName(String userName) {
		this.userName = userName;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPasswrod() {
		return passwrod;
	}

	public void setPasswrod(String passwrod) {
		this.passwrod = passwrod;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

}
```

## 3) DAO

### Interface
> UserDetailsDao.java

```java
package com.example.demo.dao;

import java.util.List;

import com.example.demo.model.UserDetails;

public interface UserDetailsDao {
	List<UserDetails> getUserDetails();
	UserDetails findUserById(String userId);
}
```

### Implements
> UserDetailsImpl.java

```java
package com.example.demo.dao;

import java.util.List;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Repository;

import com.example.demo.model.UserDetails;

@Repository
public class UserDetailsImpl implements UserDetailsDao {

	private final Logger logger = LoggerFactory.getLogger(this.getClass());

	@PersistenceContext
	private EntityManager entityManager;

	@SuppressWarnings("unchecked")
	@Override
	public List<UserDetails> getUserDetails() {
		// TODO Auto-generated method stub
		String jpql = "SELECT user FROM UserDetails as user ORDER BY user.userId DESC";
		List<UserDetails> result = entityManager.createQuery(jpql).getResultList();
		for (UserDetails userDetails : result) {
			logger.info(userDetails.getName());
		}
		return (List<UserDetails>)result;
	}

	@Override
	public UserDetails findUserById(String userId) {
		// TODO Auto-generated method stub
		return entityManager.find(UserDetails.class, userId);
	}

}
```

## 4) Service

### Interface
> UserDetailsService.java    

```java
package com.example.demo.service;

import java.util.List;

import com.example.demo.model.UserDetails;

public interface UserDetailsService {
	List<UserDetails> getUsers();
}
```

###  Implements
> UserDetailsServiceImpl.java

```java
package com.example.demo.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.demo.dao.UserDetailsDao;
import com.example.demo.model.UserDetails;

@Service
public class UserDetailsServiceImpl implements UserDetailsService {

	@Autowired
	private UserDetailsDao userDao;

	@Override
	public List<UserDetails> getUsers() {
		// TODO Auto-generated method stub
		return userDao.getUserDetails();
	}

}
```

## 5) Controller
> UserController.java

```java
package com.example.demo.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.example.demo.model.UserDetails;
import com.example.demo.service.UserDetailsService;

@Controller
@RequestMapping("/user")
public class UserController {

	@Autowired
	private UserDetailsService userService;

	@CrossOrigin(origins = "http://localhost:3000")
	@GetMapping(value = "/list", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
	public ResponseEntity<List<UserDetails>> getAllUser() {
		List<UserDetails> user = userService.getUsers();
		return new ResponseEntity<List<UserDetails>>(user, HttpStatus.OK);
	}

	@RequestMapping(value = "/listPage", method = RequestMethod.GET)
	public String findAll(Model model) {
		List<UserDetails> users = userService.getUsers();

		model.addAttribute("users", users);

		for(UserDetails user : users) {
			System.out.println(user.getEmail() + " " + user.getName());
		}
		return "userList";
	}
}
```
