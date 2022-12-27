---
title: "科学上网 2.0 时代"
date: 2022-12-01T18:03:00+08:00
tags: ["经验分享"] 
draft: false
toc: true
---

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202212029Dz2QB.jpg)

## 引言

鉴于最近几年国内审查越来越严格，科学上网应该成为每个人的必备技能。本文将介绍我是如何科学上网的，以及我使用了哪些工具。

## 为什么要科学上网

科学上网的目的是为了访问被审查的网站，比如 Google、YouTube、Twitter、Telegram、Discord 等。这些网站在国内无法被访问，需要科学上网才能访问。

微信现在审查也越来越严格，群内聊天，你发敏感文字、图片只有你自己能看到，其他群友无法收到消息，真是特别恶心，所以我建议大家都使用 Telegram 吧。

YouTube 也是一个非常好的而且免费的资源平台，是除了 Google 之外我用的最多的网站了。

<!--more-->

## 如何科学上网

以前我们科学上网的方式是使用 VPN，但是比较麻烦，VPN 默认是全局连接的，后来有了 Shadowsocks，改变了我们科学上网的方式，感谢 Shadowsocks 的作者。

### 原理

Shadowsocks 是一个开源的代理软件，可以让你科学上网。你需要一个服务器，然后在服务器上安装 Shadowsocks，然后在你的电脑上安装 Shadowsocks 客户端，然后在客户端上填写服务器的 IP 地址和端口号，以及密码，就可以科学上网了。

### 1.0 时代

这是 1.0 时代的上网方式，我也曾买过搬瓦工的服务器自己搭建过 Shadowsocks，但是后来发现搬瓦工的服务器速度太慢了，而且 IP 经常被墙，IP 被墙之后只能花钱换 IP 了，所以到期之后我就没续费了。

### 2.0 时代

![20221202D8n1rT](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221202D8n1rT.png!m)

后来有人开发了 Clash，Clash 是一个代理软件，可以通过一个 URL 批量订阅大量的 Shadowsocks 节点，然后你可以在 Clash 中选择你想要的节点，这样就可以科学上网了。这样单个节点被墙了，你可以选择其他节点，不会影响你的使用。我愿称之为 2.0 时代。

节点多是一个优点，另外一个优点就是我们可以通过设置规则，比方说自动屏蔽广告的 IP/域名，这样就可以让你的网络更加安全等等，可玩性非常高了。

对这个 Clash 规则感兴趣的可以去看看这个 [clash-rules](https://github.com/Loyalsoldier/clash-rules)。

## 服务与工具

### 服务端（机场）

1.0 时代你自己搭建还可以，成本低，但是使用 2.0 方式的话，就不适合自己搭建，买其他第三方服务比较好。1.0 方式自己搭建成本低，但是不稳定，2.0 方式买第三方服务成本高一点，但是稳定。

目前我使用的第三方服务是：

[![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221201h69ifO.png!m)](http://link.3li3.com/mxwljsq)

- [猫熊网络加速器](http://link.3li3.com/mxwljsq)：我用了 1 年了，节点比较多，有土耳其、阿根廷、印度等节点，方便使用其他服务拼车。注册可以免费体验 1 天，如果你不看或者很少看 YouTube，建议你选「IPLC 专线 50G 体验套餐-VIP3」13/月的套餐（50G 流量/月），强烈建议选 IPLC 系列套餐。

[![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221201aU2z5v.png!m)](http://link.3li3.com/1yuan)

- [一元机场](http://link.3li3.com/1yuan)：真的是 1/月，超级便宜，缺点就是节点不多，但是速度还是接受的。

需要声明的是：任何「机场」都有跑路的风险，所以我们买的时候可以选择按月付费，减少自己的风险。

我非常不推荐那种自己开发 App 的科学上网服务，我更喜欢这种服务和客户端分开的方式，这样可以让我们自由选择客户端，而不是被限制在某个 App 里面。

### iOS 端

iOS 端想使用科学上网就稍微有麻烦，首选那些 App 都不在国内应用商店，所以首选你需要一个美区的 Apple ID，然后在美区的 App Store 下载这些 App。

注册美区 Apple ID 其实是免费的，嫌麻烦就花钱买吧，推荐在[小火箭吧](https://xiaohuojian8.com/aff/3200.html)买。

注册参考：[【2022 年】五分钟注册美区 Apple ID，手把手教，稳定且耐用！](https://zhuanlan.zhihu.com/p/367821925)

不建议使用不认识人的 Apple ID，有风险。另外多提一句，千万不要在 iPhone 的 iCloud 登录任何不是自己的帐号，登录之后别人可以锁你设备，很危险。只需要在 App Store 登录就可以了。

我们在 App Store 登录美区的 Apple ID 之后，就可以下载我们需要的 App 了，我目前在用的 App 有：

- [Stash](https://apps.apple.com/app/stash/id1596063349)：我当前正在用的 App，功能比较强大，价格也不贵，推荐使用。
- [Shadowrocket](https://apps.apple.com/us/app/shadowrocket/id932747118)：小火箭，便宜，销量垂直领域第一了吧，现在依旧保持更新。我把它作为备用 App。

当然这两个 App 都是收费的，然后就涉及到支付问题了，目前支付宝很方便就能实现给美区 Apple ID 充值了，具体可以看[《2022 年给苹果 iPhone App Store 美区 AppleID 充值的两种安全方法（国内信用卡或支付宝）》](https://zhuanlan.zhihu.com/p/591979093)，**强烈推荐这种方式**。

另外一种就是购买兑换码的方式购买这两个 App。这里我在网上搜索了两个网站，方便你们购买：

- [桃子商店](https://shop.tz.ci/buy/13)
- [火箭少女小卖部](https://www.rocketgirls.space/product/10.html)

⚠️ 声明：我跟上面两个网站没有任何关系，风险你们自己承担。我只是通过 Google 找到的两个比较便宜的网站。

以前我是在淘宝上购买的兑换码，不过最近去淘宝上看了一下，发现店铺都下架了，而且这几个关键词都搜索不到结果了。


### Mac 端

这里只推荐 [clashX](https://github.com/yichengchen/clashX)，开源免费。我目前在用的也是这个。

当然前面提到 Stash 也出了 Mac 版本，但是价格不便宜，感觉没必要。ClashX 足够好用了。

### Windows 端

这里推荐 [clash for Windows](https://github.com/Fndroid/clash_for_windows_pkg/releases)

PS：我没有 Windows 电脑，所以我也没用过。

### Android 端

这里推荐 [ClashForAndroid](https://github.com/Kr328/ClashForAndroid)，开源免费。

PS：我没有 Android 手机，所以我也没用过。

## 总结

最近发现身边的人越来越多的开始使用科学上网，很多非互联网行业的人都开始使用，这个趋势我觉得是好的，因为这个世界上有很多好的东西，我们应该去看看。

而与此同时国内的互联网确越来越严格的自我审查，上周我把 [使用 Telegram](https://blog.forecho.com/use-telegram.html) 发到少数派上，但是被他们给删了，其实我发之前有搜索 Telegram，我是看到有搜索结果才发的，被删我也能理解，但是我还是为国内的互联网环境感到失望。


所以我把我知道的分享给大家，帮助你们更好的使用科学上网，享受自由吧。