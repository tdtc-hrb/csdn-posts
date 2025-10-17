---
title: "java的grammar"
date: 2023-08-08T02:09:28+08:00
---

- JDK8    
At least May 2026 for Eclipse Adoptium
- JDK17    
At least Oct 2027 for [Eclipse Adoptium](https://adoptium.net/support/)
- JDK21    


# Java

## Project Loom

- [All together now: Spring Boot 3.2, GraalVM native images, Java 21, and virtual threads with Project Loom,](https://spring.io/blog/2023/09/09/all-together-now-spring-boot-3-2-graalvm-native-images-java-21-and-virtual)


## lambda
我们使用实现Runnable的不同来做对比。

### java 7
```java
package my.csdn2;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class RunnableThread implements Runnable {

	public int mul(int x, int y) {
		int result = 0;

		result = x * y;

		return result;
	}

	public void run() {
		// TODO Auto-generated method stub
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
				System.out.print("x: " + i + " y: " + j + " result: ");
				System.out.println(mul(i, j));
			}
		}
	}

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// way 1
//		Thread rt = new Thread(new RunnableThread());
//		rt.start();

		// way 2
		ExecutorService exec = Executors.newCachedThreadPool();
		exec.execute(new RunnableThread());

	}

}
```

### java8
```java
package my.csdn2;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class RunnableLambdaExample {

	public int mul(int x, int y) {
		int result = 0;

		result = x * y;

		return result;
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		RunnableLambdaExample rle = new RunnableLambdaExample();

		Runnable task1 = () -> {
			for (int i = 0; i < 5; i++) {
				for (int j = 0; j < 5; j++) {
					System.out.print("x: " + i + " y: " + j + " result: ");
					System.out.println(rle.mul(i, j));
				}
			}
		};

		// way 1
		//Thread rt = new Thread(task1);
		//rt.start();

		// way 2
		ExecutorService exec = Executors.newCachedThreadPool();
		exec.execute(task1);
	}

}
```

# Scala
在Scala把类和对象明确的分开了。    
Class相当于模板；Object是Class的实现。

要测试代码必须使用main
```scala
def main(args: Array[String]) {
    ...
}
```

## Class
```scala
package com.example

class MyClass {
  def add(a:Int, b:Int):Int = {
    var result = 0
    result = a + b
    result
  }
}
```


## Object
```scala
import com.example.MyClass


object MyExec {
  val str = "Hello world!"
  val a1 = 3
  val b1 = 2

  def main(args: Array[String]) {
      val myExample = new MyClass
      val ret = myExample.add(a1, b1)
      if (ret > 4) {
        println(str)
      }
      else {
        println(ret)
      }
  }
}
```


# Reference

- [Java 8 Lambda - Runnable Example](https://www.codejava.net/java-core/the-java-language/java-8-lambda-runnable-example)
