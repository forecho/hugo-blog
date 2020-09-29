---
title: "HTTP Location"
date: 2020-09-29T17:33:00+08:00
tags: ["编程"] 
draft: false
toc: true
---

## 引言

最近在开发 [CashWarden](https://blog.forecho.com/hello-cashwarden.html)，后端使用自己最熟悉的 [Yii2 框架](https://www.yiiframework.com/)开发的。

Yii2 默认支持 RESTful 风格，有几个接口我可以直接使用默认的就可以了，非常省力。 CashWarden 使用前后端分离的方式开发的，在这过程中，我遇到了一个非常奇怪的问题，花了一点时间（差不多一天时间吧）才解决，今天这篇文章我打算分享一下我遇到的问题，以及我是怎么解决的，希望看完你也有收获。

<!--more-->

## 提要

在开发创建标签功能的时候，我使用了 HTTP POST 请求后端 `/api/tags` 接口，因为这个接口非常的标准化，Yii2 默认的 RESTFul 就够用了。

## 问题

在线上环境发现每次创建标签，要请求三个接口，而且**开发者工具的 Console 都会有一个报错信息**，会影响功能使用。本地也是三个请求，但是没有报错信息。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20200904N5f4NJ.jpg)

- 第一个请求是 POST 创建标签的请求。
- 第二个请求是 HTTP OPTIONS 请求，因为是前端请求后端，跨域了所以有这个。
- 第三个请求是 GET 标签详情的请求。

后面两个是一组请求，这个我知道，但问题是我确定我创建完标签没有请求详情接口，我一度怀疑是前端框架的锅（因为对源码不熟），是它默认帮我请求的，但是我得找到证据，唯一的疑点在于第一个请求返回的 HTTP 状态码是 302，于是我就去找资料，最后终于被我找到了。


## 出现原因

### 出现 GET 请求的原因

因为 Yii2 默认的 `CreateAction`  的最后返回在 Header 里面加入了 `Location` ，它的值就是详情的 URL 地址：

![202009295KXziS](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202009295KXziS.jpg)

HTTP POST 返回的 Header：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20200904zMS9Ar.jpg)

### 本地没报错，线上代码报错的原因

- Angular 本地开发使用是代理方式请求后端接口的，即通过配置 `proxy.conf.json` 达到效果。但是部署的时候不能用这种方式，只能通过配置 Nginx 代理的方式请求后端接口，这就导致本地没有这个问题，上线才出现。
- 多请求一次详情也没关系，无非就多耗一点资源罢了（前端没用上这个数据）。但是因为我配置了一个 Nginx 代码规则，URL 有一个替换规则，最后导致请求这个 URL 的时候其实是 404 的（不存在的 URL），这就导致了报错，报错就导致了前端弹窗形式的表单虽然提交成功了，但是没有自动关闭。这就影响功能了。

## 解决

既然找到了问题，不是前端的锅，而是后端的锅那就好解决问题了。我的解决方式就是继承 `CreateAction` ，然后重写 `run` 方法。删掉加 Header 的那行代码，改为使用 `findOne()` 查数据，返回结果。

## 总结

其实遇到这个问题，主要还是自己 HTTP 协议不熟导致的，只知道 302 的重定向跳转，但是不知道 Header 如果有 `Location` 浏览器会自动请求的知识。

与此类似的知识点就是前端跨域的 HTTP OPTIONS 请求。

## 扩展阅读

- [header (PHP 手册)](https://www.php.net/manual/zh/function.header.php)
- [HTTP Location (维基百科)](https://zh.wikipedia.org/wiki/HTTP_Location)
