---
title: "计算圆的面积和周长"
description: "使用c++类"
date: 2025-01-07T08:08:08+08:00
---
- [Eclipse IDE for C/C++ Developers](https://www.eclipse.org/downloads/packages/)
- [msys2](https://www.msys2.org/) or [cygwin](https://cygwin.com/)
```
automake, gcc-core, gcc-g++, gdb, make
```

## Circular 类
- Circular.h
```c++
#pragma once
class Circular
{
public:
    Circular(double pi);
    virtual ~Circular();

    double PI;

    double getArea(double radius);

    double getCircumference(double radius);
};
```
- Circular.cpp
```c++
#include "Circular.h"

Circular::Circular(double pi)
{
    PI = pi;
}

Circular::~Circular()
{

}

double Circular::getArea(double radius) {
    return PI * (radius * radius);
}

double Circular::getCircumference(double radius) {
    return PI * (radius * 2);
}
```

## Create Project
File -> New -> C/C++ Project    
"C++ Managed Build"

- Project type
```bash
Executable: Hello World C++ Project
```
- Toolchains
```bash
cygwin GCC / mingw GCC
```
- testCir2.cpp
```c++
#include <iostream>
#include <stdlib.h>  // atof
#include <string.h>  // strlen

#include "Circular.h"

using namespace std;  // C2065: 'cout' and 'endl'

// https://stackoverflow.com/a/8032108
const wchar_t *GetWC(const char *c)
{
    const size_t cSize = strlen(c)+1;
    wchar_t* wc = new wchar_t[cSize];
    mbstowcs (wc, c, cSize);

    return wc;
}

int main(int argc, char* argv[]) // https://stackoverflow.com/a/780147
{
    const double Pi = 3.14;
    double dRadius = 3;
    if (argc > 1) {
        dRadius = atof(argv[1]);
        // https://en.cppreference.com/w/c/string/wide/wcstof
        // const wchar_t* p = GetWC(argv[1]);
        // wchar_t* end;
        // dRadius = wcstof(p, &end);
    }
    // 输入的半径
    cout << "Entered radius: " << dRadius << endl;

    Circular *circular = new Circular(Pi);
    // 面积
    double dArea = circular->getArea(dRadius);
    cout << "Area: " << dArea << endl;
    // 周长
    double dCircumference = circular->getCircumference(dRadius);
    cout << "Circumference: " << dCircumference << endl;

    return 0;
}
```

## run application
- msys2
```
libwinpthread-1.dll
libgcc_s_seh-1.dll
libstdc++-6.dll
```
- cygwin
```
cygwin1.dll
cyggcc_s-seh-1.dll
cygstdc++-6.dll
```

### [Msys2](https://www.msys2.org/docs/environments/#__tabbed_1_1)
|Name |C Library |C++ Library|
|-|-|-|
|UCRT64  |ucrt|libstdc++|
|MINGW64 |msvcrt|libstdc++|

- update
```
# Update package mirrors
pacman -Sy pacman-mirrors
```
- msvcrt
```
pacman -S make gdb \
          mingw-w64-x86_64-gcc \
          mingw-w64-x86_64-autotools
```
- ucrt
```
pacman -S make gdb \
          mingw-w64-ucrt-x86_64-gcc
          mingw-w64-ucrt-x86_64-autotools
```
