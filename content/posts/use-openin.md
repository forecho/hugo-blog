---
title: "使用 OpenIn 为你的链接设置专属浏览器"
date: 2023-02-15T18:29:00+08:00
tags: ["效率", "工具控"]
draft: false
toc: true
---

![](https://img.forecho.com/jnjEy0.jpg)

## 引言

在日常使用中，你是否会需要这种情况，你使用的浏览器有两个帐户，一个是你的个人帐户，一个是你的工作帐户，工作帐户的 Chrome 登录的是工作的 GitHub 帐户，个人帐户的 Chrome 登录的是你私人的 GitHub 帐户，用于区分。

在工作沟通的 IM 软件中，你希望点击链接时，能够使用工作帐户的 Chrome 打开，而不是个人帐户的 Chrome 打开。不然同事发给你的私有仓库链接，你打开的时候，会提示你没有权限。

今天在 SetAPP 上我就发现了一款软件，叫做 OpenIn，可以帮助你解决这个问题。稍微配置一下，然后全自动完成你的需求。

<!--more-->

## OpenIn

### 安装

[OpenIn](https://loshadki.app/openin4/) 是一款高级链接处理的程序，如果你是 SetAPP 用户，可以在 SetAPP 上直接下载安装，如果不是，需要在 App Store 上购买，价格为 11.99 美元。

安装之后需要把它设置为默认浏览器，然后就可以使用了。

### 配置

我的使用场景是：

- 我绝大部分时间都之用 Chrome 浏览器
- 两个帐户，一个是个人帐户，一个是工作帐户

![](https://img.forecho.com/ByXFIh.png)

所以我 Applications 里面配置了两个 Chrome，一个是个人帐户的 Chrome，一个是工作帐户的 Chrome。

![](https://img.forecho.com/XPawNG.png)

配置多个 Chrome 帐户的关键是里面的 `Profile` 这个值，要通过 `chrome://version` 地址获取这个值，更详情的说明可以看[官方文档](https://loshadki.app/openin4/020-web/#edit-application)，我刚开始没看文档浪费了一点时间。

我的需求是：**使用从 Lark.app 的所有链接都使用工作帐户的 Chrome 打开，其他任何链接都使用我的私人帐户的 Chrome 打开。**

这个需求我是通过 Rule 来实现的。

默认的图形化 Rule 配置无法实现我的需求，所以我使用了它提供的 `Custom Script` Rule 来实现，修改默认的 Rule，打开 `Custom Script`，然后把下面的代码复制进去，保存即可。

```js
(function() {
    // the list of available apps to send link to
    let apps = ctx.getApps()

    // if the source app (where link is opened is Slack, 
    // keep only Safari as visible app for OpenIn
    // and because we will have only one app left, 
    // OpenIn will just open the Safari
    if (ctx.getSourceApp().path.startsWith("/Applications/Lark.app")) {
        apps.forEach(function (app) {
            app.visible = (app.name == "Chrome Work")
        })
        return
    }
	apps.forEach(function (app) {
        app.visible = (app.name == "Chrome Forecho")
    })
})()
```

OpenIn 的规则还是挺强大的，还可以根据不同的域名匹配不同的浏览器，更多功能你们自己看文档或者自己试试吧。

## 最后

其实这个需求我一直都有，每次都是人工操作，很烦，没想到昨天在 SetAPP 上发现了这个软件，正好解决了我的问题。

从来没想到过会有软件能够帮助我解决这个问题，真是太棒了。