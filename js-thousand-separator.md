---
title: "千位分隔符"
date: 2021-11-18T02:18:39+08:00
---

# 1. 字符串替换

```js
/**
 * 字符串替换函数 - js
 * expStr:  查找的字符串
 * string:  整个字符串
 * repStr:  替换的字符串
 */
function replacStr(expStr, string, repStr) {
	let result = "";
	let regex;
	if (parseInt(expStr) > 0)  // 数字
		regex = new RegExp(expStr, "g");
	else			// 非数字
		regex = new RegExp("\\" + expStr, "g");
	result = string.replace(regex, repStr);
	return result;
}
```
# 2. 千分符
例如，货币7867321.55，怎样使用js来分割他呢？
## 0. 分离小数和整数
```js
let index = strCny.indexOf(".");
if ( index != 0) {
    strNum = strCny.substring(0, index);
    strDec = strCny.substr(index - iLenCny);

    strCny = strNum;
} 
```

## 1. 转换为string
```js
let strCny = cny.toString();
```

## 2. 转化为数组
```js
let array = strCny.split("");
```

## 3. 循环数组
1. 由个位开始
2. 每3位，加个逗号
3. 控制count和len
>循环一次计数一次；
每增加个逗号，长度（len）+1

## 4. 转换string

```js
let result = array.join("");
```
并输出
```js
console.log(result);  
//alert(result);
```

# 完整代码
money.html
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta content="text/html; charset=utf-8" />
<title>千位分割符</title>
</head>

<body>
<br>
<br>
数字：
<input type="text" id = "edtData" />
<br>
<br>

结果：<input type="text" id = "edtView" />
<br>
<br>
<input type="button" id="btnOk" value="执行" onClick="thousand_separator(document.getElementById('edtData').value)" />
</body>

<script>  
function thousand_separator(cny) {
    let strCny = cny.toString();
    let array = strCny.split("");
    let strNum = "";
    let strDec = "";

    let result = "";

    let iLenCny = strCny.length;
    let index = strCny.indexOf(".");
    if ( index > 0) {
        strNum = strCny.substring(0, index);
        strDec = strCny.substr(index - iLenCny);

        strCny = strNum;
    }

    let len = strCny.length;
    let count = 0;
    for (let j = len; j > 0; j--) {
        count++;
        if (count % 4 == 0) {
            array.splice(j, 0, ",");
            count++;
            len++;
        }
    }

    result = array.join("") + strDec;

    let edtView = document.getElementById("edtView");
    edtView.value = " ";
    edtView.value = result;
}
</script>
</html>
```
