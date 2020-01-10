---
title: "Git 初使用"
date: 2013-10-11T22:30:00+08:00
tags: ["git", "github"] 
draft: false
toc: true
---

- Mac 安装 Git，软件下载地址：<https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>

- 第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录： 

```sh
$ git config --global user.name "forecho"
$ git config --global user.email caizhenghai@gmail.com
```

- 建立 SSH key 可以让你在你的电脑和 Git @ OSC 之间建立安全的加密连接。

```sh
$ ssh-keygen -t rsa -C "caizhenghai@gmail.com"
```

- 查看你的 public key，并把他添加到 SSH keys 中 <https://github.com/settings/ssh>

```sh
$ cat ~/.ssh/id_rsa.pub
```

- Clone 项目

```sh
$ git clone https://github.com/iiYii/getyii.git
```

- 复制文件到这个目录下，然后跟踪新的文件：

```sh
$ git add .    # 跟踪所有改动过的文件
$ git add -u    # 只加修改过的文件, 新增的文件不加入.
$ git add -i     # 进入互动模式
```

- 提交变更：

```sh
$ git commit -m "xxxx"
$ git commit -a -m 'xxxx'
```

>**commit和commit -a的区别:**
commit -a相当于：
第一步：自动地add所有改动的代码，使得所有的开发代码都列于index file中
第二步：自动地删除那些在index file中但不在工作树中的文件
第三步：执行commit命令来提交 ​

- 推送到服务器上：

```sh
$ git push
```

- 恢复单个文件

```sh
$ git checkout -- hello.rb
```

远程服务器覆盖当前的改动：

```sh
$ git checkout -f
```

- 添加被忽略的文件以及文件夹

```sh
$ git add -f 文件路径
```

- 服务器更新本地

```sh
$ git pull
```

- git如何查看某一个文件的详细提交记录

```sh
$ git log -p filename
```


- 配置常用别名

```sh
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

- 使用 vim 作为默认编辑器

```sh
git config --global core.editor "vim"
```

- 忽略 `new mode old mode` 修改

```sh
git config core.filemode false
```
