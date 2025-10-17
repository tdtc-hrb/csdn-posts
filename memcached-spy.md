---
title: "使用spymemcached操作memcached"
date: 2020-04-11T21:08:39+08:00
---

[offical site](https://github.com/ragnor/simple-spring-memcached/wiki/Getting-Started)

# pom
```xml
<dependency>
  <groupId>com.google.code.simple-spring-memcached</groupId>
  <artifactId>spymemcached-provider</artifactId>
  <version>4.1.3</version>
</dependency>
```

# app config
> applicationContext.xml

```xml
<import resource="simplesm-context.xml" />
    <aop:aspectj-autoproxy />

    <bean name="defaultMemcachedClient" class="com.google.code.ssm.CacheFactory">
        <property name="cacheClientFactory">
          <bean name="cacheClientFactory" class="com.google.code.ssm.providers.spymemcached.MemcacheClientFactoryImpl" />
        </property>
        <property name="addressProvider">
          <bean class="com.google.code.ssm.config.DefaultAddressProvider">
            <property name="address" value="127.0.0.1:11211" />
          </bean>
        </property>
        <property name="configuration">
          <bean class="com.google.code.ssm.providers.CacheConfiguration">
            <property name="consistentHashing" value="true" />
          </bean>
        </property>
    </bean>
```

# code
> service layer

```java
@Override
@ReadThroughSingleCache(namespace = "carnum", expiration = 300)
public List<TrainOrder> getCarList(String year, String month, @ParameterValueKeyProvider int trainNumber) {
	// TODO Auto-generated method stub
	return trainOrderDao.getCarnumberList(year, month, trainNumber);
}
```
