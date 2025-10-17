---
title: "动态生成table"
description: "根据JSON数据，使用TS/JS生成HTML的表格"
date: 2021-10-02T14:26:39+08:00
---

# 一、JSON数据源
> [JSON](http://www.bejson.com/go.html?u=http://bejson.com/aboutJSON.html)
>>
```
JSON是一种取代XML的数据结构,和xml相比,它更小巧但描述能力却不差,由于它的小巧所以网络传输数据将减少更多流量从而加快速度,

那么,JSON到底是什么?

JSON就是一串字符串 只不过元素会使用特定的符号标注。

{} 双括号表示对象

[] 中括号表示数组

"" 双引号内是属性或值

: 冒号表示后者是前者的值(这个值可以是字符串、数字、也可以是另一个数组或对象)

所以 {"name": "Michael"} 可以理解为是一个包含name为Michael的对象

而[{"name": "Michael"},{"name": "Jerry"}]就表示包含两个对象的数组

当然了,你也可以使用{"name":["Michael","Jerry"]}来简化上面一部,这是一个拥有一个name数组的对象
```

详细介绍请参见[W3SCHOOL](http://www.w3school.com.cn/json/)
[JSON规范](http://www.ecma-international.org/ecma-262/5.1/)

## 1. students
```js
var students = [
  {"cName": "class 1", "ID": "1001", "name": "李X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1002", "name": "张X威", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1003", "name": "于X洋", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1004", "name": "张X扬", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1005", "name": "周X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1006", "name": "王X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1007", "name": "李X朋", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 1", "ID": "1008", "name": "邬X春", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 5", "ID": "1009", "name": "李X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
  {"cName": "class 5", "ID": "1010", "name": "孙X丽", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]}
];
```

# 二、函数设计
> 所谓的动态就是使用删表和写表的操作。

## 1. 删除表格
> 删除除了表头之外的数据。

```js
CourseInf.deleteTable = function (table) {
	var mBody = table.getElementsByTagName('tbody');

	var len = table.rows.length;

	for (var i = 1; i < len; i++) {
		table.removeChild(mBody[1]);
	}
};
```

## 2. 写表格
```js
CourseInf.writeTable = function (obj, table) {
	var array = [];
	var className = obj.value;

	var iAcc = 0;
	var rowNum = 0;
	var str = "";

	for (var i = 0; i < CourseInf.gradeArrays.length; i++) {

		for (var key in CourseInf.gradeArrays[i]) {
			if (CourseInf.gradeArrays[i][key] == className){
				CourseInf.flag = true;
				rowNum++;
			}
			else {
				if (CourseInf.flag) {
					array[key] = CourseInf.gradeArrays[i][key];
				}
			}

		}
		CourseInf.flag = false;		

		if (rowNum != 0) {
			for (var j in array) {
				str = array[j];
				CourseInf.classArrays[iAcc] = str;

				iAcc++;
			}
			CourseInf.insertTable(4, 1, CourseInf.classArrays, table);

		}

		rowNum = 0;		
	}

	CourseInf.iCount = 0;
};
```
如果数组中某个键值（cName）等于className则执行：标志为真；并且行数+1操作。
否则，判断标志是否为真？为真读取数据。
```js
if (CourseInf.gradeArrays[i][key] == className){
  CourseInf.flag = true;
  rowNum++;
}
else {
  if (CourseInf.flag) {
    array[key] = CourseInf.gradeArrays[i][key];
  }
}
```
行数不等于0时，插入表格数据
```js
CourseInf.insertTable = function (colNum, rowNum, array, table) {
	var mBody = document.createElement("tbody");

	for (var i = 0; i < rowNum; i++) {
		var mTr = document.createElement("tr");

		var j = 0;
		for (j = 0; j < colNum; j++) {
			var mCell = document.createElement("td");
			var mText = document.createTextNode(array[j + CourseInf.iCount]);

			mCell.appendChild(mText);
			mTr.appendChild(mCell);
		}
		CourseInf.iCount += j;
		mBody.appendChild(mTr);
	}
	table.appendChild(mBody);
};
```

## 3. 删除重复项
我们在读取班级列表的时候，希望一个班级只能出现一次。
所以，我们要处理多个“class 1”和“class 5”的问题。
```js
for (var i = 0; i < array.length; i++) {
  obj[array[i]] = true;
}
```

> obj[array[i]] = true;     
也许这么看就容易懂了     
> obj[key] = true;

# 附
## dynamicTable.html
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
	<title>动态创建表格</title>
	<script type="text/javascript">
		var students = [
			{"cName": "class 1", "ID": "1001", "name": "李X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1002", "name": "张X威", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1003", "name": "于X洋", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1004", "name": "张X扬", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1005", "name": "周X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1006", "name": "王X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1007", "name": "李X朋", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 1", "ID": "1008", "name": "邬X春", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 5", "ID": "1009", "name": "李X", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]},
			{"cName": "class 5", "ID": "1010", "name": "孙X丽", "course": ["英语", "高数"], "elective": ["政治经济学", "哲学"]}
		];
	</script>

	<script type="text/javascript" src="dynamicTable.js" ></script>

	</head>

</html>
```

- js version
```html
	<body onLoad="CourseInf.loadData(students)">
		<select id="className" onChange="CourseInf.readClasses(this)">
		</select>
		<br>     
		<table id="tb1" border="1">
		  <tr>
		    <th>学号</th>
		    <th>姓名</th>
		    <th>必修课</th>
		    <th>选修课</th>
		  </tr>
		</table>

	</body>
```

- ts version
```html
	<body onLoad="CourseInf.prototype.loadData(students)">
		<select id="className" onChange="CourseInf.prototype.readClasses(this)">
		</select>
		<br>     
		<table id="tb1" border="1">
		  <tr>
		    <th>学号</th>
		    <th>姓名</th>
		    <th>必修课</th>
		    <th>选修课</th>
		  </tr>
		</table>

	</body>
```

## js
> tsc dynamicTable.ts

- dynamicTable.ts
```js
class CourseInf {
    gradeArrays : any;
    static classArrays : any =	[];

    iCount : number;
    flag : boolean;

    constructor() {
        this.gradeArrays =  [];

        this.iCount 	=	0;
        this.flag 		= 	false;
    }

    deleteTable(table : any) {
        let mBody = table.getElementsByTagName('tbody');
    
        let len = table.rows.length;
    
        for (let i = 1; i < len; i++) {
            table.removeChild(mBody[1]);
        }
    }

    insertTable(colNum : number, rowNum : number, array : any, table : HTMLElement) {
        let mBody = document.createElement("tbody");
    
        for (let i = 0; i < rowNum; i++) {
            let mTr = document.createElement("tr");
    
            let j : number = 0;
            for (j = 0; j < colNum; j++) {
                let mCell = document.createElement("td");
                let mText = document.createTextNode(array[j + this.iCount]);
    
                mCell.appendChild(mText);
                mTr.appendChild(mCell);
            }
            this.iCount += j;
            mBody.appendChild(mTr);
        }
        table.appendChild(mBody);
    }

    writeTable(obj : any, table : any) {
        let array : any = [];
        let className : object = obj.value;
    
        let iAcc : number = 0;
        let rowNum : number = 0;
        let str : string = "";
    
        for (let i = 0; i < this.gradeArrays.length; i++) {
    
            for (let key in this.gradeArrays[i]) {
                if (this.gradeArrays[i][key] == className){
                    this.flag = true;
                    rowNum++;
                }
                else {
                    if (this.flag) {
                        array[key] = this.gradeArrays[i][key];
                    }
                }
    
            }
            this.flag = false;
    
            if (rowNum != 0) {
                for (let j in array) {
                    str = array[j];
                    CourseInf.classArrays[iAcc] = str;
    
                    iAcc++;
                }
                this.insertTable(4, 1, CourseInf.classArrays, table);
    
            }
    
            rowNum = 0;		
        }
    
        this.iCount = 0;
    }

    readClasses(obj : object) {
        let table = document.getElementById("tb1");
    
        this.deleteTable(table);
        this.writeTable(obj, table);
    }

    loadData(array : any) {
        let data = [];
        let vals = [];
    
        this.gradeArrays = array;
    
        let obj = document.getElementById("className") as any;
    
        // json 中键值为"cName"的value值放到数组中
        for (let i = 0; i < array.length; i++) {
            for (let j in array[i]) {
                if (j == "cName")
                    data.push(array[i][j]);			
            };
        }
    
        let _toObj = function (array : any) {
            let obj = new Array();
    
            for (let i = 0; i < array.length; i++) {
                obj[array[i]] = true;
            }
    
            return obj;
        };
    
        let _keys = function (obj : object) {
            let array = new Array();
    
            for (var i in obj) {
                if (obj.hasOwnProperty(i)) {
                    array.push(i);
                };
            }
    
            return array;
        };
    
        let _uniq = function (array : any) {
            return _keys(_toObj(array));
        }
    
        // 删除重复项
        vals = _uniq(data);
    
        // Traverse Array
        for (let i = 0; i < vals.length; i++) {
            obj.options.add(new Option(vals[i], vals[i]));
        }
    
    }

}
```


- dynamicTable.js

```js
/*
 * 课程信息
 * @author xiaobin
 */

var CourseInf = {
	gradeArrays :   [],
	classArrays :	[],

	iCount 		:	0,
	flag 		: 	false,
};


CourseInf.deleteTable = function (table) {
	var mBody = table.getElementsByTagName('tbody');

	var len = table.rows.length;

	for (var i = 1; i < len; i++) {
		table.removeChild(mBody[1]);
	}
};

CourseInf.insertTable = function (colNum, rowNum, array, table) {
	var mBody = document.createElement("tbody");

	for (var i = 0; i < rowNum; i++) {
		var mTr = document.createElement("tr");

		var j = 0;
		for (j = 0; j < colNum; j++) {
			var mCell = document.createElement("td");
			var mText = document.createTextNode(array[j + CourseInf.iCount]);

			mCell.appendChild(mText);
			mTr.appendChild(mCell);
		}
		CourseInf.iCount += j;
		mBody.appendChild(mTr);
	}
	table.appendChild(mBody);
};

CourseInf.writeTable = function (obj, table) {
	var array = [];
	var className = obj.value;

	var iAcc = 0;
	var rowNum = 0;
	var str = "";

	for (var i = 0; i < CourseInf.gradeArrays.length; i++) {

		for (var key in CourseInf.gradeArrays[i]) {
			if (CourseInf.gradeArrays[i][key] == className){
				CourseInf.flag = true;
				rowNum++;
			}
			else {
				if (CourseInf.flag) {
					array[key] = CourseInf.gradeArrays[i][key];
				}
			}

		}
		CourseInf.flag = false;		

		if (rowNum != 0) {
			for (var j in array) {
				str = array[j];
				CourseInf.classArrays[iAcc] = str;

				iAcc++;
			}
			CourseInf.insertTable(4, 1, CourseInf.classArrays, table);

		}

		rowNum = 0;		
	}

	CourseInf.iCount = 0;
};

CourseInf.readClasses = function (obj) {
	var table = document.getElementById("tb1");

	CourseInf.deleteTable(table);
	CourseInf.writeTable(obj, table);
};

CourseInf.loadData = function (array) {
	var data = [];
	var vals = [];

	CourseInf.gradeArrays = array;

	var obj = document.getElementById("className");

	// json 中键值为"cName"的value值放到数组中
	for (var i = 0; i < array.length; i++) {
		for (var j in array[i]) {
			if (j == "cName")
				data.push(array[i][j]);			
		};
	}

	var _toObj = function (array) {
		var obj = new Array();

		for (var i = 0; i < array.length; i++) {
			obj[array[i]] = true;
		}

		return obj;
	};

	var _keys = function (obj) {
		var array = new Array();

		for (var i in obj) {
			if (obj.hasOwnProperty(i)) {
				array.push(i);
			};
		}

		return array;
	};

	var _uniq = function (array) {
		return _keys(_toObj(array));
	};

	// 删除重复项
	vals = _uniq(data);

	// Traverse Array
	for (var i = 0; i < vals.length; i++) {
		obj.options.add(new Option(vals[i], vals[i]));
	};

};
```
