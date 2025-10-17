---
title: " 使用Ts/Js计算天数"
date: 2021-10-01T02:16:39+08:00
---
在JS练习题中，有这么一道：

请计算某天是这一年的第几天。

首先，我们要看此年是否闰年
```js
var IsLeapYear = function (year) {
  var flag = false;

  if ( ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0) )
    flag = true;

  return flag;
};
```
其次，累加每月天数。
```js
for (var i = 0; i < month - 1; i++) {
  iDay += months[i];
}
```
> 其中months为数组
```js
var months = new Array(31, day2, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);
```

day2如果是闰年赋值29，否则28。
```js
if (IsLeapYear(iYear)) {
  day2 = 29;
} else {
  day2 = 28;
}
```

# js code
> tsc calDay.ts

## calDay.ts
```js
class MY {

    DayOfYear(year: number, month: number, day: number) : number {
        let day2 : number = 0;
        let iDay : number = 0;

        let date = new Date();
        date.setFullYear(year, month, day);
        let iYear : number = date.getFullYear();

        let IsLeapYear = function (year: number) {
            let flag : boolean = false;

            if ( ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0) )
                flag = true;

            return flag;
        };

        if (IsLeapYear(iYear)) {
            day2 = 29;
        } else {
            day2 = 28;
        }

        let months = new Array(31, day2, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);

        for (let i = 0; i < month - 1; i++) {
            iDay += months[i];
        }

        iDay += day;

        this.SetInputVal(iDay);

        return iDay;
    }

    SetInputVal (value: any) {
        let val : any = <HTMLInputElement>document.getElementById("daySum");
        val.value = value;
    }

}
```

## calDay.js
```js
/*
 * xiaobin in SYSIT schools completed
 */
var MY = {};

MY.DayOfYear = function(year, month, day) {
    var day2 = 0;
    var iDay = 0;

    var date = new Date();
    date.setFullYear(year, month, day);
    var iYear = date.getFullYear();

    var IsLeapYear = function (year) {
        var flag = false;

        if ( ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0) )
            flag = true;

        return flag;
    };

    if (IsLeapYear(iYear)) {
        day2 = 29;
    } else {
        day2 = 28;
    }

    var months = new Array(31, day2, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);

    for (var i = 0; i < month - 1; i++) {
        iDay += months[i];
    }

    iDay += day;

    SetInputVal(iDay);

    return iDay;
};

function SetInputVal (value) {
    var val = document.getElementById("daySum");
    val.value = value;
};
```

# calDay.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Example01</title>
<script type="text/javascript" src="calDay.js" > </script>
</head>

<body>
<p>
    </br>
    <input type="text" id="daySum" >
    <input type="button" id="test02"  value="cal day" onClick="MY.DayOfYear(2000, 2, 8)">
    </br>
    <input type="button" id="test03"  value="cal day (type-script)" onClick="MY.prototype.DayOfYear(2000, 2, 8)">
</p>

</body>
</html>
```
