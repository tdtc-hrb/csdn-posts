---
title: "dispatcher - Asynchronous"
description: "flux与redux的不同"
date: 2022-04-10T17:26:39+08:00
---

# 1. Program Flow

## 1) Synchronous
![flux-unidir-ui-arch](https://github.com/tdtc-hrb/csdn/raw/master/images/flux-unidir-ui-arch.png)

## 2) Asynchronous
![redux-unidir-ui-arch](https://github.com/tdtc-hrb/csdn/raw/master/images/redux-unidir-ui-arch.png)

# 2. redux-saga
>  about CQRS: [Event-sourcing](http://codebetter.com/gregyoung/2010/02/20/why-use-event-sourcing/)

![Event-sourcing](https://github.com/tdtc-hrb/csdn/raw/master/images/redux-unidir-ui-arch.png)

## 1) Install
```bash
npm install --save redux-saga
```
[examples](https://github.com/redux-saga/redux-saga/tree/master/examples)

# Reference
- [UNIDIRECTIONAL USER INTERFACE ARCHITECTURES - André Staltz](https://staltz.com/unidirectional-user-interface-architectures.html)
- [Clarified CQRS](http://udidahan.com/2009/12/09/clarified-cqrs/)
