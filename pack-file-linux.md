---
title: "打包文件"
description: "在Linux下使用光盘（ISO）和压缩文件来打包文件"
date: 2023-02-10T02:20:39+08:00
---
 > 取消目录所有权限
 ```bash
chmod -R 777 /tmp/etc
 ```

# 一、ISO文件
```bash
mkisofs -r -o etc.iso /tmp/etc
```

# 二、压缩文件

## 1. zip
```bash
zip -r etc.zip /tmp/etc
```

## 2. tar
> 仅打包，不压缩！
```bash
tar -cvf /tmp/etc.tar /etc
```

- transform
```
--transform s/OLD_NAME/NEW_NAME/
```


### gz
```bash
tar -zcvf /tmp/etc.tar.gz /etc
```
transform:
```bash
tar -zcvf /tmp/etc.tar.gz /etc --transform s/etc/myetc/
```

### bz2
```bash
tar -jcvf /tmp/etc.tar.bz2 /etc
```
transform:
```bash
tar -jcvf /tmp/etc.tar.bz2 /etc --transform s/etc/myetc/
```

# FAQ

## This does not look like a tar archive
```bash
#gzip -d xxxx.tar.gz
#tar -xf xxxx.tar
```

## gzip: stdin: not in gzip format
```bash
$ tar xvf xxxx.tar.gz
```


# 参考文章
- [18 Tar Command Examples in Linux](https://www.tecmint.com/18-tar-command-examples-in-linux/)
```bash
c – Creates a new .tar archive file.
v – Verbosely show the .tar file progress.
f – File name type of the archive file.
```
1. To create a compressed <strong>gzip</strong> archive file we use the option as <strong style="color: red;">z</strong>.
2. To create highly compressed tar file we use option as <strong style="color: red;">j</strong>.

- [Linux下更改目录及其下的子目录和文件的访问权限](http://blog.csdn.net/chenjiiinliang/article/details/7288173)
- [linux zip/unzip命令](http://www.cnblogs.com/lucyjiayou/archive/2011/12/25/2301046.html)
- [Linux下制作ISO与刻录ISO](http://blog.csdn.net/clozxy/article/details/5748539)
