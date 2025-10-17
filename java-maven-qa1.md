---
title: "maven的QA"
description: "stackoverflow.com"
date: 2022-05-15T10:08:08+08:00
---

# 1. org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER
Q:
```bash
Classpath entry org.eclipse.m2e.MAVEN2_CLASSPATH_CONTAINER will not be exported or published. Runtime ClassNotFoundExceptions may result.  
```
A:
```bash
$mvn eclipse:clean
```

## reference
- [what-is-org-eclipse-m2e-maven2-classpath-container-and-how-do-i-make-it-work-in](https://stackoverflow.com/questions/37624859/what-is-org-eclipse-m2e-maven2-classpath-container-and-how-do-i-make-it-work-in)

# 2. no main manifest attribute java jar
Q:
```bash
$java -jar cliDemo-0.0.1-SNAPSHOT.jar
no main manifest attribute, in cliDemo-0.0.1-SNAPSHOT.jar
```
A:
> for win:
```bash
java -cp cliDemo-0.0.1-SNAPSHOT.jar;C:\Users\xiaobin\.m2\repository\commons-cli\commons-cli\1.4\commons-cli-1.4.jar my.csdn9.ConsoleLauncher --help
```

## Reference
- [how-to-run-jar-files-depend-on-jar-files](https://stackoverflow.com/questions/41035499/how-to-run-jar-files-depend-on-jar-files)

# 3. CoreException: Could not calculate build plan: Plugin org.apache.maven.plugins:maven-compiler-plugin
Q:
```bash
CoreException: Could not calculate build plan: Plugin org.apache.maven.plugins:maven-compiler-plugin:3.1 or one of its dependencies could not be resolved: Failure to find org.apache.maven.plugins:maven-compiler-plugin:jar:3.1 in http://repository.jboss.org/nexus/content/groups/public was cached in the local repository, resolution will not be reattempted until the update interval of jboss-public-repository-group has elapsed or updates are forced
```
A:
找到maven库目录，进入：~\.m2\repository\org\apache\maven\plugins\maven-compiler-plugin      
删除3.1文件夹

## Reference
- [cnblog - liaojie](http://www.cnblogs.com/liaojie970/p/5509760.html)


# 4. Proxy
conf/settings.xml

```xml
  <!-- proxies
   | This is a list of proxies which can be used on this machine to connect to the network.
   | Unless otherwise specified (by system property or command-line switch), the first proxy
   | specification in this list marked as active will be used.
   |-->
  <proxies>
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>127.0.0.1</host>
      <port>8580</port>
    </proxy>
  </proxies>
```


# 5. How to add Oracle JDBC driver in your Maven local repository
## down [ojdbc6.jar](http://www.oracle.com/technetwork/database/enterprise-edition/jdbc-112010-090769.html)

## exec
```bash
mvn install:install-file -Dfile=D:/war/lib/ojdbc6.jar -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0 -Dpackaging=jar
```

## pom.xml
```xml
<!-- ojdbc6.jar example -->
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc6</artifactId>
    <version>11.2.0</version>
</dependency>
```
