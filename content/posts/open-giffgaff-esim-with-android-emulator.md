---
title: "用安卓模拟器开通 giffgaff eSIM 的记录"
date: 2026-06-15T16:58:35+08:00
tags: ["经验分享", "eSIM", "giffgaff"]
draft: false
toc: true
---

![](https://r2.imgant.com/2026/06/15/b58745b7.png)

## 引言

6 月 14 日晚上，看到几条推文和一个 YouTube 教程后，我跟着折腾了一圈，最后用电脑上的安卓模拟器申请到了英国 giffgaff eSIM。

我的主力机只有一台 iPhone 16 Pro Max，双实体卡槽。一个卡槽放老号，用来收短信和接电话；另一个卡槽放宽带送的流量卡，日常上网。第三方 eSIM 实体卡要插进 iPhone，就要占一个卡槽。

这次先把 giffgaff eSIM 申请下来，等 eSTK Plus+ 到货后再写入实体卡使用。Max 当时缺货，Plus+ 对我这个场景已经够用。

<!--more-->

## 我的需求

我需要一个海外手机号，主要用来收验证码、注册海外服务，以及做账号备用。giffgaff 的优势是英国号码、支持 eSIM、维护成本低，中文圈也有不少实操记录。

giffgaff 官方 eSIM 流程主要走 App，App 会检查设备是否支持 eSIM。我这种只有国行 iPhone、还想写入第三方实体 eSIM 卡的用户，要先拿到 eSIM 激活信息，再写入 eSTK。

## 我一开始想用旧安卓手机

我手上有一台十多年前的安卓手机，最开始想通过云萝卜这类工具配合 HookEuicc 来搞定。

实际卡在了系统版本上。老安卓系统太旧，HookEuicc 一直安装失败，后面的 LSPosed、模块生效、giffgaff App 检测都走不下去。

这个坑比较典型。教程里写“准备一台安卓手机”，真正执行时还要看系统版本、Magisk、LSPosed、HookEuicc 这些环节。我的旧手机第一步就卡住了，换电脑安卓模拟器更省时间。

## 最后跑通的是电脑安卓模拟器

我参考的 YouTube 教程标题很直接：《无需手机，全电脑操作：英国 giffgaff 电话卡手把手申请教程》。视频里走的是电脑安卓模拟器路线，核心工具大致是：

- MuMu 模拟器
- Magisk
- LSPosed
- HookEuicc / 相关 giffgaff 模块
- giffgaff App

大致流程是：

1. 在电脑上安装安卓模拟器。
2. 安装 Magisk，确认 root 环境正常。
3. 安装 LSPosed，并启用 HookEuicc 相关模块。
4. 安装 giffgaff App，登录账号。
5. 在 App 里申请 eSIM。
6. 保存拿到的 eSIM 激活信息。

我就是沿着这条路跑通的。相比旧安卓手机，模拟器环境可控，版本也更容易匹配教程。

一般会看到类似这种格式：

```text
LPA:1$SM-DP+地址$Activation Code
```

这里面的 `SM-DP+地址` 和 `Activation Code` 要保存好。后面写入 eSTK、9eSIM、XeSIM 这类实体 eSIM 卡时都要用到。

## 为什么我最后选 eSTK Plus+

我最开始在 9eSIM、XeSIM、eSTK 之间纠结。调研之后，我觉得 eSTK 性价比最高：

- 价格合适。
- 容量够用，我主要放 giffgaff，再加少量旅行 eSIM。
- iPhone 用户可用，STK 菜单和跨平台工具都能覆盖我的使用方式。

Max 的容量更大，更适合经常切换很多国家和运营商 eSIM 的人。可惜我下单时 Max 没货，所以直接买了 Plus+。

我的使用方案很简单：

1. eSTK Plus+ 到货后，把 giffgaff eSIM 写进去。
2. iPhone 里继续保留老号，保证短信和电话稳定。
3. 日常国内上网继续用宽带送的流量卡。
4. 需要 giffgaff 或旅行 eSIM 时，把第二卡槽换成 eSTK。

这也是双实体卡 iPhone 的现实取舍。老号承载短信和电话，我会固定留在手机里；第二卡槽在流量卡和 eSTK 之间按场景切换。

## 后续计划

1. 只有 iPhone 的用户，先把 eSIM 激活信息拿到手，再考虑写入实体 eSIM 卡。
2. 老安卓手机容易卡在系统版本和模块兼容性上，电脑安卓模拟器更省时间。
3. 电脑安卓模拟器更适合照着教程复现，环境可控，排错成本低。
4. eSTK Plus+ 对我这种轻度使用场景已经够用。
5. 双卡槽 iPhone 的核心取舍是保留老号，第二卡槽在流量卡和 eSTK 之间切换。

下一步等 eSTK Plus+ 到货。真正写入、切换、收短信是否稳定，还要等实体卡到手之后再验证。

## 参考资料

- [YouTube：无需手机，全电脑操作：英国 giffgaff 电话卡手把手申请教程](https://www.youtube.com/watch?v=0UnJjASaqE8)
