---
title: "最小实现redux - 分解redux"
description: "starter project"
date: 2022-04-10T08:26:39+08:00
---
project: https://gitee.com/xiaobin80/starter

# application
> app.js
## include createStore
```js
import { createStore } from 'redux'
```

## Establish reducer
```js
const reducerName = (state, action) => {
    state;
}
```
## invoke createStore()
```js
const store = createStore(reducerName);
```

# html
> public/index.html
```xml
<!DOCTYPE html>
<html>
  <head>
    <title>The Complete Redux Book - Example Application</title>
  </head>

  <body>
    <div id="app">
      Redux is running: <span id="counter"></span>
    </div>

    <script type="text/javascript" src="../app.js"></script>
  </body>
</html>
```