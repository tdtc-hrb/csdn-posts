---
title: "git操作 部分1"
description: "远程仓库回滚, 删除github发布, 修改commit信息"
date: 2022-02-02T01:38:08+08:00
---

# 一、远程仓库回滚
## 1. 硬性
```bash
git reset --hard HEAD~1
```
- "~1"表示向前回滚1个提交。
- "~2"表示向前回滚2个提交，以此类推。

## 2. 强制推送
- https
```bash
git push -f --set-upstream origin master
```
### ssh
- [SourceTree](https://community.atlassian.com/forums/Sourcetree-questions/How-to-quot-force-quot-push/qaq-p/718539)
```
1. Go to Tools > Options.Select the Git tab.
2. Check the box for Enable Force Push.
3. (Recommended) Check Use Safe Force Push (--force-with-lease) if available, 
4. as it prevents overwriting someone else's work.
```

# 二、删除github发布
```bash
git tag -d [tag]
git push origin :[tag]
```
例如：
```bash
git tag -d v0.3.1
git push origin :v0.3.1
```

# 三、修改commit信息
可以修改最后一次提交的信息.

进入git-cmd或git-bash：
```bash
git commit --amend
```

About：
> git-bash和git-cmd的区别
>> bash是linux风格的命令行，可以使用windows和linux的命令；
>> cmd是windows风格的命令行，可以使用windows命令；
>>> 此外，还有一个最大的不同点，git-cmd是天生就带了系统的PATH环境变量，
>>> 这一点会导致如果你运行某些脚本的时候，如果依赖某些安装的程序，
>>> 如python、node这些，git-bash就会提示不存在对应的命令，
>>> 而git-cmd可以正确运行。
 

# 四、合并分支
## 1. checkout
```bash
git checkout dev
git checkout master
```

## 2. merge
```bash
git merge origin/dev
```

# 五、设置user
```bash
git init
```
```bash
git config user.name "xiaobin80"
git config user.email "veic_2005@163.com"
```


# Issues
Git-2.39.1-64-bit + TortoiseGit-2.14.0.0-64bit
## safe directory
Starting in Git v2.35.3, safe directory checks can be disabled, which will end all the "unsafe repository" errors

It will add the following setting to your global .gitconfig(C:\Users\<username>) file:
```
[safe]
	directory = *
```


# Reference
- [How to resolve git's “not something we can merge” error](https://stackoverflow.com/questions/16862933/how-to-resolve-gits-not-something-we-can-merge-error/16862934#16862934)
-[I cannot add the parent directory to *safe.directory* in Git](https://stackoverflow.com/a/71904131)
