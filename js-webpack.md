---
title: "webpack的使用"
description: "怎样用webpack-dev-server来部署js"
date: 2021-11-08T15:26:39+08:00
---

# 安装webpack

```bash
npm install -g webpack-dev-server
```

```bash
npm install --save webpack-cli
```

# pack info
> package.json

```js
{
  "name": "redux-book-started",
  "version": "1.0.0",
  "description": "Sample Redux Application",
  "main": "app.js",
  "scripts": {
    "start": "webpack-dev-server --hot --progress  --port 8181"
  },
  "author": "Boris Dinkevich & Ilya Gelman",
  "license": "MIT",
  "dependencies": {
    "jquery": "3.6.0",
    "redux": "4.1.2"
  },
  "devDependencies": {
    "webpack": "5.64.1",
    "webpack-cli": "^4.9.1",
    "webpack-dev-server": "4.5.0"
  }
}
```

## config info
> webpack.config.js

```js
module.exports = {
  mode: 'development',
  entry: './app.js',
  output: {
    filename: './app.js'
  }
};
```

## run
```js
npm install
npm start
```

code on [Github](https://github.com/tdtc-hrb/starter)
