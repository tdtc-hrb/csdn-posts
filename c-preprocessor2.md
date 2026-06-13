---
title: "c预处理器"
description: "Function attributes - Align and inline"
date: 2026-06-12T12:08:08+08:00
---
**Note: Use MSVC to interpret alignment and inline.**

### Default
```
#ifndef _FX
#define _FX       // ALIGNED NOINLINE
#endif
```
### aligned
- vs2013
```
#ifndef ALIGNED
#define __declspec(align(16))
#endif
```
- vs2015+
```
#ifndef ALIGNED
#define alignas(16)
#endif
```
### Noline
```
#ifndef NOINLINE
#define NOINLINE  __declspec(noinline)
#endif
```

## Ref
- [version choice](https://gcc.gnu.org/onlinedocs)
- [Function Attributes - 6.5.0](https://gcc.gnu.org/onlinedocs/gcc-6.5.0/gcc/Common-Function-Attributes.html#Common-Function-Attributes)
- [Function Attributes - 8.5.0](https://gcc.gnu.org/onlinedocs/gcc-8.5.0/gcc/Common-Function-Attributes.html#Common-Function-Attributes)
- [How can I tell gcc not to inline a function?](https://stackoverflow.com/questions/1474030/how-can-i-tell-gcc-not-to-inline-a-function)
- [noinline - MSVC](https://learn.microsoft.com/en-us/cpp/cpp/noinline)
- [align - vs2015-](https://learn.microsoft.com/en-us/cpp/cpp/align-cpp)
- [alignas - vs2015+](https://learn.microsoft.com/en-us/cpp/cpp/alignas-specifier)
