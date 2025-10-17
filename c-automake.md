---
title: "生成makefile"
description: "在Cygwin中执行测试"
date: 2023-08-08T02:20:39+08:00
---
- automake
- manual make

# automake
注意: am文件是需要手工创建的!

## ac文件
生成configure.scan文件, 并重命名为configure.ac
```bash
$ autoscan
$ mv configure.scan configure.ac
```

### 修改配置
注意: 所有新修改的值都需要方括号([])

- 程序属性
```bash
AC_INIT([hex2int], [v0.1], [veic_2005@163.com])
```

- 目录结构
如果只有本目录, 使用空值
```bash
AM_INIT_AUTOMAKE
```

- 检查编译器
[C compiler](http://www.adp-gmbh.ch/misc/tools/configure/configure_in.html#ac_prog_cc):
```bash
AC_PROG_CC
```
[C++ compiler](http://www.adp-gmbh.ch/misc/tools/configure/configure_in.html#ac_prog_cxx):
```bash
AC_PROG_CXX
```

- [输出Makefile.in](https://www.gnu.org/software/autoconf/manual/autoconf-2.71/html_node/Configuration-Files.html)
```bash
AC_CONFIG_FILES([Makefile])
```

- 检查system头文件
```bash
AC_CHECK_HEADERS([string.h])
```


## 生成Makefile
```bash
aclocal
autoconf
```

### 自定义宏(可选)
在ac file中的定义：
```bash
AC_CONFIG_HEADERS([config.h])
```
执行：
```bash
autoheader
```

### 输出Makefile
```bash
automake --add-missing
```

输出Makefile:
```bash
./configure
```

## Example
源代码参见[十六进制转十进制](https://tdtc-hrb.github.io/csdn/post/c-string-h2i/)

- main.c    
只保留主程序入口
- hex2int.c    
保留整个程序的结构
- hex2int.h    
函数名声明

### Makefile.am
```bash
AUTOMAKE_OPTIONS=foreign
bin_PROGRAMS=hex2int
hex2int_SOURCES=main.c hex2int.c
include_HEADERS=hex2int.h
```

### configure.ac
```bash
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.71])
AC_INIT([hex2int], [v0.1], [veic_2005@163.com])
AM_INIT_AUTOMAKE
AC_CONFIG_SRCDIR([hex2int.c])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CC

# Checks for libraries.

# Checks for header files.
AC_CHECK_HEADERS([string.h])

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
```






# 手动Makefile
> 小项目可以。
注意：所有的命令前必须为TAB，不能使空格！

```bash
#https://stackoverflow.com/questions/1484817/how-do-i-make-a-simple-makefile-for-gcc-on-linux
#https://stackoverflow.com/questions/14109724/makefile-missing-separator/14109796
#https://www.gnu.org/software/make/manual/html_node/Pattern-Examples.html#Pattern-Examples
#https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html#Automatic-Variables


TARGET = hex2int
CC = gcc
CFLAGS = -g -Wall


all:$(TARGET)

OBJECTS = $(patsubst %.c, %.o, $(wildcard *.c))
HEADERS = $(wildcard *.h)

%.o: %.c $(HEADERS)
	$(CC) -c $(CFLAGS) $< -o $@

$(TARGET): $(OBJECTS)
	$(CC) $(OBJECTS) -Wall -o $@

clean:
	-rm -f *.o
	-rm -f $(TARGET)
```

## 多个 include 路径
Specifies a directory dir to search for included makefiles.
```
LIB=-L/usr/informix/lib/c++
INC=-I/usr/informix/incl/c++ -I/opt/informix/incl/public

default:    main

main:   test.cpp
        gcc -Wall $(LIB) $(INC) -c test.cpp
        #gcc -Wall $(LIB) $(INC) -I/opt/informix/incl/public -c test.cpp

clean:
        rm -r test.o make.out
```


# 参考文档
- [Linux c 开发 - Autotools使用详细解读](https://blog.csdn.net/initphp/article/details/43705765)
- [Makefile 中引用多个 include 路径](https://www.cnblogs.com/liujx2019/p/11205195.html)
- [老版本init automake错误](https://www.gnu.org/software/automake/manual/automake.html#Modernize-AM_005fINIT_005fAUTOMAKE-invocation)
- [make OPTIONS](https://man7.org/linux/man-pages/man1/make.1.html)
