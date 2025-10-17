---
title: "c预处理器"
description: "华氏和摄氏"
date: 2020-05-04T12:08:08+08:00
---
我们在编写C程序时常常与预处理器打交道。

比如下列华氏转摄氏程序

```c
/*
 * celsius.c
 *
 *  Created on: 2012-11-14
 *      Author: xiaobin
 */

#include <stdio.h>

#define FREEZING_PT (16.0 *	\
				2)
#define SCALE_FACTOR (5.0 / 9.0)

int main(void)
{
	float fahrenheit, celsius;
	printf("Enter Fahrenheit temperature: ");
	scanf("%g", &fahrenheit);
	printf("%g\n", fahrenheit);
	celsius = (fahrenheit - FREEZING_PT) * SCALE_FACTOR;
	printf("Celsius equivalent is: %g", celsius);
	return 0;
}
```

1. 预处理器换行
  指令总是在第一个换行符处结束，除非明确地指明要继续。

  如果想在下一行继续指令，我们必须在当前行的末尾使用\字符。
```c
#define FREEZING_PT 16.0 *	\
				2
```

2. 替换列表的小括号
  尽量使用小括号把替换列表括起来，这样便于阅读

3. 带参数的预处理

  上述程序可以使用一个带参数的预处理来替代程序中的处理语句
```c
#define FA2CE(f) ((f - FREEZING_PT) * SCALE_FACTOR)
```

# 附：带参预处理器程序
```c
/*
 * celsius.c
 *
 *  Created on: 2012-11-14
 *      Author: xiaobin
 */

#include <stdio.h>

#define FREEZING_PT (16.0 *	\
				2)
#define SCALE_FACTOR (5.0 / 9.0)
#define FA2CE(f) ((f - FREEZING_PT) * SCALE_FACTOR)

int main(void)
{
	float fahrenheit, celsius;
	printf("Enter Fahrenheit temperature: ");
	scanf("%g", &fahrenheit);
	printf("%g\n", fahrenheit);
	celsius = FA2CE(fahrenheit);
	printf("Celsius equivalent is: %g", celsius);
	return 0;
}
```
