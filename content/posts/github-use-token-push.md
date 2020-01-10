---
title: "GitHub 单独设置某个项目的配置信息"
date: 2016-02-15T15:08:00+08:00
tags: ["github", "git"] 
draft: false
toc: true
---
## 场景

我们先这种情况的谈谈这个使用场景，你不一定用得到这个方法。

A 拿 B 电脑打算使用一段时间，A 用 git clone 了一份自己的代码，想用 B 电脑写一些代码。但是 B 平常也写代码，B 电脑上有一套自己的 git 环境.

那么问题来了，我们要怎样在不修改 B 电脑全局环境的情况下使用 GitHub？

## 单独为项目配置用户名和 Email

<!--more-->

针对每个项目，单独设置用户名和邮箱，设置方法如下：

```sh
cd ～/test // git检出目录
git config user.name "your_name"
git config user.email "your_email"
```
相应的变量要替换成你自己的信息。

## 单独设置 token 推送代码

我记得以前只要是使用 HTTPS 协议的 GitHub 项目，推送的时候会问你账号密码，但是也不知道这次是不是我使用
的这台电脑已经有 ssh 的原因，push 的时候提示我403错误，没有权限，我项目使用的 URL 明明也是 HTTPS。

Google 了半天，最后终于找到解决办法，就是单独生成一个 token，Git remote 的 URL 设置成 

```sh
git remote set-url origin https://<token>@github.com/forecho/blog.git
```
其中的 token 你要自己去 GitHub 生成，地址是 <https://github.com/settings/tokens>

然后你就可以愉快的 push 代码了，不需要输入账号密码。

参考文章：[Easier builds and deployments using Git over HTTPS and OAuth](https://github.com/blog/1270-easier-builds-and-deployments-using-git-over-https-and-oauth)

## 总结

出现以上原因只是因为对 Git 以及 GitHub 命令不算很熟悉。

