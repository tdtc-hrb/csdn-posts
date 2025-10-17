---
title: "倒序排列字符串"
description: "两种方式:指针和数组"
date: 2020-05-04T14:08:08+08:00
---
- 指针    
首先，我们要给出一个长度够用的字符串指针（dest）。

 然后，递减这个指针到0。在递减的过程中，dest累加而我们预设的字符串累减。这种方式是指针标准操作方式。
我使用了预设字符串指针地址+这个字符串长度变量这种方式（因为递减长度也是在递减）。
- 数组    
介绍省略。




代码如下：
```c
#include <string.h>
#include <stdlib.h>
#include <stdio.h>


int main(int argc, char* argv[])
{
    char *src = "hello,world";
    int len = strlen(src);

    char *dest = NULL;
    dest = (char *)malloc(len + 1);
    // dest = (char *)malloc(len + 1); // standard: 要为'\0'分配一个空间

    char *d = dest;
    // char *s = &src[len - 1];        // standard: 指向最后一个字符

    /* One way */
    while (len-- != 0) {
        *d++ = *(src + len);           // one way
        //*d++ = *s--;                 // standard:
    }

    *d = 0;                            // standard: 尾部要加0
    /* Two way */
    /*
    int i = 0;
    while (len-- != 0) {
        dest[i] = src[len];            // two way
        i++;
    }
    */
    printf("%s\n", dest);

    free(dest);                        // standard:  使用完，应当释放空间，以免造成内存汇泄露

    return 0;
}

```
