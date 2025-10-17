---
title: "重构asyn"
description: "使用create-react-app重建redux-sage的asyn"
date: 2022-06-10T07:26:39+08:00
---

# 0. pre
## 生成基本框
```bash
npm install -g create-react-app
create-react-app async
```

## clone repo
https://github.com/redux-saga/redux-saga.git

![final effect 1](https://github.com/tdtc-hrb/csdn/raw/master/images/dropdown1-rjs.png)

![final effect 2](https://github.com/tdtc-hrb/csdn/raw/master/images/dropdown2-frd.png)

# 1. copy directorys
Copy the examples/async/src from the redux-saga to your async/src.

# 2. rename main.js
> async/src    

Rename main.js to index.js

# 3. move files
> async/src

App.css && logo.svg
->
>  async/src/containers

App.css && logo.svg

# 4. modify containers/App.js
Add the following two blocks of code.
```js
import logo from './logo.svg';
import './App.css';
```
```js
<div className="App">
    <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <h1 className="App-title">Welcome to React</h1>
    </header>
    ...
</div>
```

# 5. install dependencies
> redux-saga v1.1.3

```js
npm install --save web-vitals
npm install --save react-redux
npm install --save redux-saga
npm install --save isomorphic-fetch
```
