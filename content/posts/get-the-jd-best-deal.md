---
title: "薅京东羊毛"
date: 2021-06-24T18:09:00+08:00
tags: ["经验分享"] 
draft: false
toc: true
---

## 更新

此方法已经不再适用，这种方式违反了 GitHub Actions 使用条款，账号 Github Actions 有被禁用的风险，别问我是为什么知道的？🤦‍♂️ 

而且 GitHub Actions 如果被禁用，申诉的话，要一个月左右，😭 他们工单处理效率太低了。但是你可以在自己服务器上使用 docker 方式部署。

## 引言

本篇文章主要分享「如何通过脚本实现自动签到等功能获取京东京豆？」，京豆可以在京东购物的时候抵现金，1000 京东抵扣 10 块钱。平台会员一个月能自动获取 1000 左右的京豆吧，如果你是 PLUS 会员，会有更多。

本篇教程需要懂程序，主要面向程序员，第一个需要花点时间配置和熟悉，后面每个月也需要花几分钟更新一下就可以了。

<!--more-->

## 教程

### Fork 代码

源码在 [forecho/scripts-JD](https://github.com/forecho/scripts-JD) ，你需要先 Fork 一份。

跑这个脚本有好几种方法，我使用的是 GitHub Action，我也推荐大家使用这种方式，免费，简单好用。你基本上不用操作什么。

### 配置 Cookie

Fork 代码之后，找到项目的「Settings」->「Secrets」，然后点击 `New repository secret` 添加 `JD_COOKIE` 环境变量

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20210624SwmqXY.png)

`JD_COOKIE` 的值是通过先登录你自己账号，获取 cookie 再过滤重组得到的。具体操作步骤请看：

- [浏览器获取京东 cookie 教程](https://github.com/forecho/scripts-JD/blob/master/backUp/GetJdCookie.md)
- [浏览器插件获取京东 cookie 教程](https://github.com/forecho/scripts-JD/blob/master/backUp/GetJdCookie2.md)

我们都知道 cookie 一般都是有有效期，一般这个步骤 30 天之后需要再重新操作一遍。

### 配置通知

这个虽然不是必填，但是有了通知才放心，才能在 cookie 过期之后第一时间及时处理。

通知具体方案请看文档 [《下方提供使用到的 Secrets 全集合》](https://github.com/forecho/scripts-JD/blob/master/githubAction.md#%E4%B8%8B%E6%96%B9%E6%8F%90%E4%BE%9B%E4%BD%BF%E7%94%A8%E5%88%B0%E7%9A%84-secrets%E5%85%A8%E9%9B%86%E5%90%88)。我用的 telegram 通知，感觉最方便了，效果如下：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20210624i9nBbD.png)

这个根据你自己的情况按需配置就可以了。

## 最后

这个脚本太强大了，不仅能自动而且还可以配置多个京东账号，虽然每天获取的京豆看上去不多，但是积水成渊，一个月免费获得 10 块钱不香吗？

而且配置起来也花不了多少时间。