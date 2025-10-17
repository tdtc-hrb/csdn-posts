---
title: "简单使用vi"
description: "Linux默认的文本编辑器为vi, 相当于Windows的记事本"
date: 2023-03-10T09:28:08+08:00
---
The default editor that comes with the UNIX operating system is called vi (visual editor).

The UNIX vi editor is a full screen editor and has two modes of operation:
- Command mode commands which cause action to be taken on the file, and
- Insert mode in which entered text is inserted into the file.

# Insert mode
- 正常使用
```bash
$ vi myfile
```

- 恢复使用
```bash
$ vi -r myfile
```
恢复系统崩溃时正在编辑的文件名


要进行文字编辑工作, 按“i”即进入
```bash
i
```

## 移动光标
使用方向键来进行位置的移动。


## 删除文字
使用“Backspce”键，删除光标前面的文字。    
使用“Delete”键，删除光标后面的文字。

## 完成编辑
要退出编辑状态, 按“ESC”。


# Command mode
注意：每当冒号（:)是键入的。这种类型的命令是通过按<Return>（或<Enter>）键完成的。

- 设置行号:
```bash
:set nu
```

## 查找文字
|command|description|
|-|-|
|/string|search forward for occurrence of string in text|
|?string|search backward for occurrence of string in text|
|n|move to next occurrence of search string|
|N|move to next occurrence of search string in opposite direction|

- forward    
向前查找特定的文字，输入“/要搜索的文字”
```bash
:/string
```

- backward
向后查找特定的文字，输入“?要搜索的文字”
```bash
?string
```

- next
> 相当于Windows系统的F3功能键
继续(按原来的模式: 向前or向后)查找，按“n”

- next 2
继续查找，按“N”    
按原来的模式相反:    
```
:
```
按N后，就变成了向后查找。
同样
```
?
```
按N后，就变成了向前查找。


## To Exit vi
首先，按ESC退出编辑模式。

### 保存文件
输入“wq”完成保存并退出。
```bash
:wq
```

### 强退
如果我们只想退出并且不保存文件
```bash
:q!
```

# Ref
[Basic vi Commands](https://www.cs.colostate.edu/helpdocs/vi.html)
