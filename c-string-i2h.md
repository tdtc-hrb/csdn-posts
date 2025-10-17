---
title: "十进制转十六进制"
description: "c"
date: 2022-05-07T00:08:08+08:00
---

工作原理：
> 规则是从低位到高位
被除数，除以16, 直到商小于16。     
将余数加上余商，就是16进制数。

- 被除数
1. 等于转换数
2. 等于商    
  如果商大于16，就是被除数。  

# 手算
10进制：1566

  (1) 1566 ÷ 16 = 97    余数为14    
  (2) 97   ÷ 16 = 6     余数为1    

16进制：第1个余数(14) + 第2个余数(1) + 余商(6)  = 0x61E


# code

```c
/*
 * Int2hex.c
 *
 *  Created on: 2010-07-20
 *      Author: xiaobin
 */
#include <stdio.h>
#include <stdlib.h>

void int2hex(unsigned long long input, char output[]);

int main(int argc, char* argv[])
{
    int iDigit = 12 - 2; // 2205071409 // date: 2022.5.7 14:09
    char out[10];
    int k;
    unsigned long long input = 0;

    if (argc > 1)
        input = atoll(argv[1]);

    int2hex(input, out);

    printf("%s%llu%s", "Integer: ", input, " Hex: 0x");
    for (k = 0; k < iDigit; k++) {
        char ch = out[k];
        printf("%c", ch);
    }
    printf("\n");

    return 0;
}

void int2hex(unsigned long long input, char output[]) {
    int i = 9;
    int j;

    while (((input % 16) != 0) || (input > 15)) {
        char tempCh;

        switch (input % 16) {
            case 0:
                 tempCh = '0';
                 break;
            case 1:
                 tempCh = '1';
                 break;
            case 2:
                 tempCh = '2';
                 break;
            case 3:
                 tempCh = '3';
                 break;
            case 4:
                 tempCh = '4';
                 break;
            case 5:
                 tempCh = '5';
                 break;
            case 6:
                 tempCh = '6';
                 break;
            case 7:
                 tempCh = '7';
                 break;
            case 8:
                 tempCh = '8';
                 break;
            case 9:
                 tempCh = '9';
                 break;
            case 10:
                 tempCh = 'A';
                 break;
            case 11:
                 tempCh = 'B';
                 break;
            case 12:
                 tempCh = 'C';
                 break;
            case 13:
                 tempCh = 'D';
                 break;
            case 14:
                 tempCh = 'E';
                 break;
            case 15:
                 tempCh = 'F';
                 break;
        }
        output[i] = tempCh;
        input = input / 16;
        i--;
    }

    for (j = i; j > -1; j--)
        output[j] = '0';
}
```
