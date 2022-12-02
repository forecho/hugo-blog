---
title: "Mac 安装 Docker"
date: 2015-07-26T22:55:00+08:00
tags: ["Docker", "Mac"] 
draft: false
toc: true
---

## 前言

前几天 MacBook 拿去修了，回来时候貌似覆盖的方式重装了一回系统了。反正就是 Apache 环境都没了，索性借这次机会使用一下 Docker 吧。

## 安装

根据[官网的文档](https://docs.docker.com/mac/step_one/)安装 Docker，如果一切正常的话就是下载一个软件，双击傻瓜式的安装就可以了。

## 下载太慢怎么办？

刚才找到一个国内的[镜像](http://get.daocloud.io/#install-boot2docker)，速度还不错，推荐。

## 安装失败报错怎么办？

```sh
> docker run hello-world
> FATA[0000] Post http:///var/run/docker.sock/v1.18/containers/create: dial unix /var/run/docker.sock: no such file or directory. Are you trying to connect to a TLS-enabled daemon without TLS?
  FAIL
```

<!--more-->

解决办法参考[[DevOp] Mac上使用Docker](http://chinhui-blog.logdown.com/posts/263035-note-on-a-mac-using-the-docker) 和 [Mac OS X Yosemite 10.10.1 安装 Docker](http://blog.csdn.net/delphiwcdj/article/details/41780063)

## 测试是否安装成功

参考这篇文章：[[DevOp] Mac上使用Docker (2)](http://chinhui-blog.logdown.com/posts/263794-note-docker-on-mac-2)