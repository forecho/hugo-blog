---
title: "本地多 GitHub 账号使用"
date: 2023-04-17T13:33:00+08:00
tags: ["git", "github"]
draft: false
toc: true
---

## 引言

如果你在公司也试用 Github 托管代码的话，你可能会遇到一个问题，本地同时使用你的个人账号和公司账号。

那么本篇文章就为你分享如何在本地同时使用多个 Github 账号。


### SSH 配置

## 生成工作需要的 ssh

这里默认你已经有了个人的 ssh 密钥，如果没有的话，你需要先生成个人的 ssh 密钥。

```shell
ssh-keygen -t rsa -b 4096 -C "forecho@work.com" -f ~/.ssh/work
```

<!--more-->

### 配置 host

```shell
vim ~/.ssh/config
```

```
Host *
    ServerAliveInterval 60

#default github
Host github.com
    HostName github.com
    IdentityFile ~/.ssh/id_rsa

Host w.github.com
    HostName github.com
    IdentityFile ~/.ssh/work
```

## 使用

如果要使用 work 密钥，则这样 clone 代码

```shell
git clone git@w.github.com:work/xxx.git
```

## 配置 Git User 问题

### 先取消全局 可选操作

```shell
git config --global --unset user.name 
git config --global --unset user.email
```

### 配制个人 SSH

```shell
vim ~/.gitconfig
```

```
[user]
    name = forecho
    email = echo@forecho.com

[includeIf "gitdir:~/work/"]
    path = ~/work/.gitconfig
```


### 工作目录单独设置


```shell
vim ~/work/.gitconfig
```

```
[user]
    name = work
    email = forecho@work.com
```

## 检查

```shell
git config --list
```