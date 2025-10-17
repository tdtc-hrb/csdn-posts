---
title: "使用go-bindata"
date: 2022-04-10T02:10:39+08:00
---

# 安装

```bash
$ go get -u github.com/go-bindata/go-bindata/...
```

# generation bindata.go

```bash
$ cd ~/go/src/github.com/xiaobin80/go-micro-services/
$ go-bindata data/...
$ mv bindata.go ./data/
```

# 修改包名

```go
package main
```
---》

```go
package xxx
```
> 此例xxx为data。

# Reference

- [go-bindata](https://github.com/go-bindata/go-bindata)
- [go-micro-services](https://github.com/harlow/go-micro-services)
