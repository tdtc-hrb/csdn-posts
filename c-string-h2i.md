---
title: "十六进制转十进制"
description: "c"
date: 2021-05-01T05:08:08+08:00
---

工作原理：     
>　16进制数 * 16<sup>n</sup> 之和就是10进制数。

# 手算
16进制: 0x61E     

0位：  E * 16<sup>0</sup> = 14     
1位：  1 * 16<sup>1</sup> = 16    
2位：  6 * 16<sup>2</sup> = 1536        

10进制：    
  14+16+1536 = 1566

# code

程序的需要两部分组成：
1. 权值计算     
16的几次方，power(16, x)
```c
unsigned long power(int a, int b)
{
  int i;
  unsigned long result = 1;
  for(i = 0; i < b; i++)
    result = result * a;

  return result;
};
```

2. 单16进制值转10进制值     
  例如：如果是F，则表示15

```c
switch (ch)
 {
   case '0':
   iCh = 0;
   break;
   case '1':
   iCh = 1;
   break;
   case '2':
   iCh = 2;
   break;
   case '3':
   iCh = 3;
   break;
   case '4':
   iCh = 4;
   break;
   case '5':
   iCh = 5;
   break;
   case '6':
   iCh = 6;
   break;
   case '7':
   iCh = 7;
   break;
   case '8':
   iCh = 8;
   break;
   case '9':
   iCh = 9;
   break;
   case 'a':
   iCh = 10;
   break;
   case 'b':
    iCh = 11;
   break;
   case 'c':
     iCh = 12;
   break;
   case 'd':
     iCh = 13;
   break;
   case 'e':
     iCh = 14;
   break;
   case 'f':
     iCh = 15;
   break;

   default:
   iCh = -1;
   break;
 }
```

为了满足，把所有输入转换为小写字母，可以使用系统函数tolower()或者我们自己写的函数toLower()
```c
int toLower(int c)
{
  if(c >= 'A' && c <= 'Z')
  {
     return c + 'a' - 'A';
  } else {
     return c;
  }
};
```

# 完整程序

## hex2int.h
```c
#ifndef _HEX2INT_H
#define _HEX2INT_H

unsigned long power(int a, int b);
int toLower(int c);
unsigned long htoi(char s[]);

#endif
```

## main.c
```c
#include <stdio.h>

#include "hex2int.h"

/* int max is 32767 */
/* "%d" only print int */
/* long max is 2147483647 */
/* "%ld" can print long type */
/* unsigned long max is 4294967295 */
/* "%lu" can print unsigned long type */

#define MB 0x0100000UL

int main(int argc, char* argv[])
{
    if(argc > 1)
        printf("long integer: %lu\n", htoi(argv[1]));
    printf("M/Byte: %ld\n", MB);
    return 0;
}
```

## hex2int.c
```c
/*
 * hex2int.c
 *
 *  Created on: 2010-07-20
 *      Author: xiaobin
 *
 */
/* #include <math.h> */
#include <string.h>
//#include <ctype.h>

unsigned long power(int a, int b)
{
    int i;
    unsigned long result = 1;
    for(i = 0; i < b; i++)
        result *= a;

    return result;
};


int toLower(int c)
{
    if(c >= 'A' && c <= 'Z')
        return c + 'a' - 'A';
    else
        return c;
};

unsigned long htoi(char *s)
{
    int i, len;
    unsigned long value, result;
    int iCh;
    unsigned long iPow;
    int pos;
    char ch;

    result = 0;
    len = 0;
    value = 0;
    iCh = -1;

    len = strlen(s);
    pos = 0;

    len -= 1;

    for(i = len; (s[i] >= '0' && s[i] <= '9')
                    || (s[i] >= 'a' && s[i] <= 'f')
                    || (s[i] >= 'A' && s[i] <= 'F'); i--) {

        ch = toLower(s[i]); /* tolower() defined in ctype.h */

        switch (ch) {
          case '0':
            iCh = 0;
            break;
          case '1':
            iCh = 1;
            break;
          case '2':
            iCh = 2;
            break;
          case '3':
            iCh = 3;
            break;
          case '4':
            iCh = 4;
            break;
          case '5':
            iCh = 5;
            break;
          case '6':
            iCh = 6;
            break;
          case '7':
            iCh = 7;
            break;
          case '8':
            iCh = 8;
            break;
          case '9':
            iCh = 9;
            break;
          case 'a':
            iCh = 10;
            break;
          case 'b':
           iCh = 11;
            break;
          case 'c':
            iCh = 12;
            break;
          case 'd':
            iCh = 13;
            break;
          case 'e':
            iCh = 14;
            break;
          case 'f':
            iCh = 15;
            break;

          default:
            iCh = -1;
            break;
        }

        iPow = power(16, pos);
        pos++;

        value = iPow * iCh;

        result += value;
    }

    return result;
};
```

## Makefile
```bash
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
