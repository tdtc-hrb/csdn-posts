---
title: "设置HikariCP"
description: "使用HikariCP替换dbcp2"
date: 2020-04-10T12:26:39+08:00
---

# 1. context.xml
> tomcat

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at
      http://www.apache.org/licenses/LICENSE-2.0
  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<!-- The contents of this file will be loaded for each web application -->
<Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
    <Resource name="jdbc/mysqlds"
        auth="Container"
        type="com.zaxxer.hikari.HikariDataSource"
        username="DBAdmin"
        password="xbfirst"
        maximumPoolSize="100"
        idleTimeout="10000"
        maxLifetime="30000"
        minimumIdle="5"
        connectionTimeout="10000"
        driverClassName="com.mysql.jdbc.Driver"
        dataSourceClassName="jdbc:mysql://127.0.0.1:3306/carnumber" />
</Context>
```

# 2. web.xml
> [github](https://github.com/xiaobin80/SpringMVC-Spring-Mybatis/commit/7adb63e00e61b6c919d18e2546e1d4c2865feead)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>car number JSP</display-name>
  <description>Spring 4.0</description>

    <!--
        - Location of the XML file that defines the root application context.
        - Applied by ContextLoaderServlet.
        -
        - Can include "/WEB-INF/dataAccessContext-local.xml" for a single-database
        - context, or "/WEB-INF/dataAccessContext-jta.xml" for a two-database context.
    -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/dataAccessContext-local.xml /WEB-INF/applicationContext.xml /WEB-INF/spring-security.xml</param-value>
        <!--
        <param-value>/WEB-INF/dataAccessContext-jta.xml  /WEB-INF/applicationContext.xml</param-value>
        -->
    </context-param>    

    <!--
        - Loads the root application context of this web app at startup,
        - by default from "/WEB-INF/applicationContext.xml".
        - Note that you need to fall back to Spring's ContextLoaderServlet for
        - J2EE servers that do not follow the Servlet 2.4 initialization order.
        -
        - Use WebApplicationContextUtils.getWebApplicationContext(servletContext)
        - to access it anywhere in the web application, outside of the framework.
        -
        - The root context is the parent of all servlet-specific contexts.
        - This means that its beans are automatically available in these child contexts,
        - both for getBean(name) calls and (external) bean references.
    -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <servlet>
        <servlet-name>carnumberJSP2</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>2</load-on-startup>
    </servlet>    

    <!-- Spring Security: Begin -->
    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- Spring Security: End -->

    <servlet-mapping>
        <servlet-name>carnumberJSP2</servlet-name>
        <!--
        <servlet-name>action</servlet-name>
        <url-pattern>*.do</url-pattern>
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>    

  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>

<resource-ref>
    <description>DB Connection</description>
    <res-ref-name>dbc/mysqlds</res-ref-name>
    <res-type>com.zaxxer.hikari.HikariDataSource</res-type>
    <res-auth>Container</res-auth>
    <res-sharing-scope>Shareable</res-sharing-scope>
</resource-ref>
</web-app>
```

# Reference
- [Tomcat数据库连接池的配置方法总结](https://www.cnblogs.com/limeiky/p/5714294.html)
