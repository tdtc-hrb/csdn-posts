---
title: "Struts2 + Spring + JPA"
description: "使用Struts2时，无法使用最新的Spring"
date: 2020-04-10T10:26:39+08:00
---

# PART I: Struts2
> Refence: [《how to create a struts2 web application》](https://struts.apache.org/getting-started/how-to-create-a-struts2-web-application.html)

## 1. pom.xml

## 1) Struts2
> struts2-spring-plugin

```xml
<dependency>
  <groupId>org.apache.struts</groupId>
  <artifactId>struts2-spring-plugin</artifactId>
  <version>${struts2.version}</version>
</dependency>
```

## 2) jsp
### servlet
```xml
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.1.0</version>
</dependency>
```

### jstl
```xml
<!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl -->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```

## 2. web.xml

- filter
```xml
<filter>
  <filter-name>struts2</filter-name>
  <filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
</filter>
```

## 3. struts.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
    "http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>

    <constant name="struts.devMode" value="true" />

    <package name="basicstruts2" extends="struts-default">
    	<action name="index">
            <result>/index.html</result>
        </action>
    </package>

</struts>
```

## 4. HomeAction extends ActionSupport
 https://struts.apache.org/getting-started/coding-actions.html

 - add class and method name
> struts.xml->action

```xml
<action name="home" class="com.example.controller.HomeAction" method="index">
    <result name="success">views/home.jsp</result>
</action>
```

## 5. home.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Basic Struts 2 Application - home</title>
</head>
<body>
	<h1>Welcome To Struts 2!</h1>
</body>
</html>
```

# PART II: Data

## 1. add spring
```xml
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>
    classpath:applicationContext.xml,
    classpath:dataAccessContext-jpa.xml
  </param-value>
</context-param>
```

## 2. applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:aop="http://www.springframework.org/schema/aop"
		xmlns:tx="http://www.springframework.org/schema/tx"
		xmlns:context="http://www.springframework.org/schema/context"
		xsi:schemaLocation="
			http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
			http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
			http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
			http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd" >

	<context:component-scan base-package="com.example.demo" />
</beans>
```

## 3. update pom.xml
- spring-orm
- hibernate-entityManager
- commons-dbcp2
- mysql-connector

## 4. jdbc.properties

### 1) update applicationContext.xml
```xml
<!-- ========================= GENERAL DEFINITIONS ========================= -->
<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
  <property name="locations">
    <list>
      <value>classpath:jdbc.properties</value>
    </list>
  </property>
</bean>
```

## 5. dataAccessContext-jpa.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN 2.0//EN" "http://www.springframework.org/dtd/spring-beans-2.0.dtd">

<beans>
	<!-- DBCP1: <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close"> -->
	<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
		<property name="driverClassName" value="${jdbc.driverClassName}"/>
		<property name="url" value="${jdbc.url}"/>
		<property name="username" value="${jdbc.username}"/>
		<property name="password" value="${jdbc.password}"/>
	</bean>

	<bean id="persistenceUnitMgr"
		class="org.springframework.orm.jpa.persistenceunit.DefaultPersistenceUnitManager">
		<property name="persistenceXmlLocations">
			<list>
				<value>classpath*:META-INF/persistence.xml</value>
			</list>
		</property>
		<property name="defaultDataSource" ref="dataSource"></property>
	</bean>

	<bean id="entityManagerFactory"
		class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="persistenceUnitManager" ref="persistenceUnitMgr"></property>
	</bean>

	<bean id="transactionMgr" class="org.springframework.orm.jpa.JpaTransactionManager">
		<property name="entityManagerFactory" ref="entityManagerFactory"></property>
	</bean>

</beans>
```

## 6. persistence.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.0" xmlns="http://java.sun.com/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd ">
  <persistence-unit name="UP_AB" transaction-type="RESOURCE_LOCAL">
   <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
   <properties>
     <property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect"/>
     <property name="hibernate.hbm2ddl.auto" value="update"/>
     <property name="hibetnate.show_sql" value="true"/>
   </properties>
  </persistence-unit>
</persistence>
```

> NOTE:
>> [org.hibernate.ejb.HibernatePersistenceProvider](https://stackoverflow.com/questions/22691252/error-deprecated-persistenceprovider-use-hibernatepersistenceprovider-instead) =》org.hibernate.jpa.HibernatePersistenceProvider

## 7. Establish CRUD



## 8. add DAO definitions
update dataAccessContext-jpa.xml
```xml
<!-- ========================= DAO DEFINITIONS: IBATIS IMPLEMENTATIONS ========================= -->
	<bean id="userDao" class="com.example.demo.dao.UserDetailsImpl"></bean>
```

## 9. add business object definitions
update applicationContext.xml
```xml
<!-- ========================= BUSINESS OBJECT DEFINITIONS ======================== -->

<bean id="user" class="com.example.demo.service.UserDetailsServiceImpl">
  <property name="userDao" ref="userDao"/>
</bean>
```

## 10. Add processing code
update HomeAction.java
```java
public String userList() {
  List<UserDetails> users = userService.getUsers();

  for(UserDetails user : users) {
    System.out.println(user.getEmail() + " " + user.getName());
  }

  ActionContext ctx = ActionContext.getContext();
      ctx.put("users", users);

  return SUCCESS;
}
```

## 11. userList.jsp
> base on JSTL

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
		"http://www.w3.org/TR/html4/loose.dtd">
<%@ taglib prefix="c"      uri="http://java.sun.com/jsp/jstl/core" %>

<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>User List Page</title>

</head>
<body>
	<table border="1">
		<tr>
		<th>User ID</th>
		<th>User Name</th>
		<th>Name</th>
		<th>Email</th>
		<th>Address</th>
		</tr>

		<c:forEach var="myData" items="${users}">
			<tr>
			<td>
				<c:out value="${myData.userId}" />
			</td>
			<td>
				<c:out value="${myData.userName}" />
			</td>
			<td>
				<c:out value="${myData.name}" />
			</td>
			<td>
				<c:out value="${myData.email}" />
			</td>
			<td>
				<c:out value="${myData.address}" />
			</td>											
			</tr>
		</c:forEach>
	</table>

</body>
</html>
```

GitHub: https://github.com/xiaobin80/struts2-spring-jpa
