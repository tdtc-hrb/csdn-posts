---
title: " 使用ts/js打印tree"
date: 2021-11-15T14:15:39+08:00
---

一棵"树"就是由空格、星号和回车组成的。    
我们用a，代表空格。
> 注意：空格在HTML编码为：“&nbsp;”。

```js
aaaa*
aaa***
aa*****
a*******
*********
aaaa*
aaaa*
aaaa*
aaaa*
aaaa*
```

首先“树头”部分由3部分组成：空格部分、左树头、右树头。    
空格就是递减的过程：    
```js
for (let i = treeRadius; i > k; i--) {
  document.write("  ");
}
```
左树头，包括中线    

```js
for (let i = 0; i < k + 1; i++) {
    document.write("*");
}
```

右树头，递增过程：

```js
for (let j = 0; j < k; j++) {
  document.write("b");
}
```
然后，“树干”部分由2部分组成：空格部分和中线部分   

空格就是按照指定的半径数“占位”：    
```js
for (let j = 0; j < treeRadius; j++) {
  document.write("  ");
}
```
> 中线就是每次循环时打印一个*    

```js
for (let j = 0; j < 1; j++) {
  document.write("*");
  document.write("</br>");
}
```
最后，使用“树高”来限定外循环。

```js
for (let i = 0; i < treeHeight; i++) {
    // something code...
}
```

实现的效果：    
```
    *
   ***
  *****
 *******
*********
    *
    *
    *
    *
    *
```


output myTree.js:
```bash
tsc myTree.ts
```

在输出的myTree.js中：

- 添加tree属性
```js
    var tree = {
        height : 0,
        radius : 0,
    };
```
- 更改方法中引用tree属性
由
```js
this.tree.height
this.tree.radius
```
变更为
```js
tree.height
tree.radius
```


# 附

## ts
- myTree.ts
```js
import { TreeModel } from './treeModel';

class MyTree {
    tree: TreeModel;

    constructor(treeHeight: number, treeRadius: number) {
        this.tree.height = treeHeight;
        this.tree.radius = treeRadius;
    }

    setTreeBody(): boolean {
        let result: boolean = false;

        for(let k = 0; k < this.tree.height; k++) {
            for (var i = this.tree.radius; i > k; i--) {
                document.write("&nbsp;");
            }
            for (var i = 0; i < k + 1; i++) {
                document.write("*");
            }
            for (var j = 0; j < k; j++) {
                document.write("b");
            }
            document.write("</br>");

            result = true;
        }

        return result;
    }

    setTreeTrunk(): boolean {
        let result: boolean = false;

        for (let i = 0; i < this.tree.height; i++) {
    
            for (let j = 0; j < this.tree.radius; j++) {
                document.write("&nbsp;");
            }
            for (let j = 0; j < 1; j++) {
                document.write("*");
                document.write("</br>");
            }
        }

        return result;
    }

}
```

- treeModel.ts
```js
export interface TreeModel {
    height: number;
    radius: number;
}
```


## Manual code
- tree.js
```
function printTree(height, radius) {
    var myTree = new MyTree(height, radius);
     // tree body
    myTree.setTreeBody();
    // tree trunk
    myTree.setTreeTrunk();
}
```

- demoTree.html
```html
<!DOCTYPE html>
<html lang="en">
 <head>
  <meta charset="UTF-8">
  <meta name="Generator" content="EditPlus®">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">
  <title>Document</title>
  <script type="text/javascript" src="tree.js" > </script>
  <script type="text/javascript" src="myTree.js" > </script>
 </head>
 <body>
  <INPUT id="tree01" onclick="printTree(5, 4)" type="button" value="tree" />
 </body>
</html>
```
