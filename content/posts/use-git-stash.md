---
title: "处理 Git 忘记切分支修改代码的情况"
date: 2015-12-15T21:56:00+08:00
tags: ["git"] 
draft: false
toc: true
---
## 场景

有时候没注意分支，直接在 master 上做开发了，但是这是团队开发的大忌。那如果发生了这种情况怎么办，也不用着急，
我教你怎么解决。

## 解决问题

假设你现在在 master 分支上已经修改了文件，那么我们可以使用下面的命令把当前未提交到本地（和服务器）的代码推入到 Git 的栈中：

```sh
$ git stash
```
查看效果：

```sh
$ git status
```

创建分支 && 切换分支：

```sh
$ git branch dev && git checkout dev
```

还原代码：

```sh
$ git stash apply
```

ok，问题解决。下面我们再补充点知识。

<!--more-->

## 总结

把当前未提交到本地（和服务器）的代码推入到 Git 的栈中：

```sh
$ git stash
```

将以前存放的代码应用回来

```sh
$ git stash apply
```

或者使用

```sh
$ git stash pop
```

区别 pop 会删除栈里面数据 apply 会保留数据

将当前的 Git 栈信息打印出来

```sh
$ git stash list
```

将你指定版本号为stash@{1}的工作取出来

```sh
$ git stash apply stash@{1}
```

将栈清空

```sh
$ git stash clear
```

## 补充

参考：['git stash' 一个强大却容易被忽视的命令](http://bbs.chinaunix.net/thread-3605709-1-1.html)
