---
title: "使用 NAS"
date: 2018-11-30T21:03:00+08:00
tags: ["nas", "经验分享"] 
draft: false
toc: true
---

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20191105105249.jpg)

## 引言

听说 NAS 有一段时间了，双十一之前整理照片的时候，NAS 再次出现在我的视线前，当天脑子一『发热』，使用 Google 恶补了很多知识。可以备份照片、下载电影，甚至可以跑 docker，想买了。

## 什么是 NAS

NAS 是 Network Area Storage 的缩写，即网络存储解决方案，说白了就是可以自己控制的网盘，外观就是一个可以装多个硬盘的机箱，局域网内下载传输速度很快，而且比网盘功能更强大一些。

## 购买方案

使用 NAS 无非就以下几种方案：

- 购买现成的 NAS ，有群晖和威联通
- 自己买硬件自己安装系统，甚至可以安装群晖的系统，俗称黑群晖。

<!--more-->

由于双十一那天时间太紧张，而我还处于『补功课』不知道要买哪款的状态，就这样错过了 [威联通（QNAP）TS-453Bmini 4G内存](https://re.jd.com/cps/item/5056259.html?dist=jd&cu=true&utm_source=kong&utm_medium=tuiguang&utm_campaign=t_1000148409_&utm_term=0eec98b7ff5c427faf17b46f1cf490d5) 历史最低价。

至于群晖，4盘位的价格太贵。最后思来想去反正自己也是搞 IT 的，那就自己组装吧，于是各种查资料，最终选的如下配置：

-  MS04 机箱（带电源） + 华擎 J4105-ITX 主板 = ￥1579.00
- 一个金士顿 8G 2400 DDR4 内存条 = ￥404.00
- 暂时只买了一个4T的希捷酷狼硬盘 = ￥770.00

以上也是这次的 NAS 所有花费，一共 ￥2753.00

PS：

- 组装硬件的时候，机箱并不是所有的功能主板都支持，所以你只要接好跳线，电源线和 SATA 能用就行了。
- NAS 主机和普通 PC 的差别就是，NAS 不需要显卡，因为需要24小时一直开着主机，所以一定要考虑是的低耗和静音。

## 系统搭建

这一步骤我是花了很多时间才搞定，原因是我没有 Windows 系统。家里用的 Mac，公司用的 Ubuntu，不得不承认 Windows 软件生态是最强大的。如果你跟我一样只有 Mac 系统下面的经验可能会对你有帮助。你需要准备以下软件或者硬件：

- 网线
- [Etcher](https://www.balena.io/etcher/)：Mac 下我找到的唯一快速制作为 USB/SD 启动盘的软件，使用非常简单。
- 一定要 Windows10 系统（Windows 7不行）。你可以使用 Parallels Desktop 本机安装 Windows 10 虚拟系统。或者你也可以使用另外一个 U盘，并且之前有烧录好的最新版老毛桃。
- U盘，用来制作群晖的系统引导盘。需要一直插着主机上，所以体积越小越好。
- [芯片无忧 ChipEasy 软件](https://www.nas2x.com/downloads/chipeasy-en-chs.10/)
- [XPEnology DSM 6.2.0 黑群晖引导文件](https://www.nas2x.com/downloads/xpenology-dsm-6-2-0.8/history)：我下载的是 6.1.7-15284 版本
- [EditPlus 编辑器](https://dl.pconline.com.cn/html_2/1/117/id=257&pn=0.html)

### 制作黑群晖的启动盘

下载好黑群晖引导文件 [synoboot.img](http://down.nas2x.com/synology/dsm/6.1/6.1.7/ds3617xs/synoboot.img) ，然后使用 Etcher 软件把 img 文件烧录到 U盘里面去，确认烧录的是U盘，注意备份资料，而且会格式化U盘的。

### 『洗白』群晖

- 进入 Windows 10 系统，安装好 芯片无忧 ChipEasy 和 EditPlus 软件。
- 使用 芯片无忧 ChipEasy 识别 U盘的 VID 和 PID，并记下数字。
- 找到U盘根目录下的 grub.cfg 文件，使用 EditPlus 编辑器打开。
- 修改 vid 和 pid 的数字值。PS：0x 不要修改，只需要修改后面的4位。

改完之后就完成了，简单吧。但是这一步我花费了很多时间，重蹈覆辙了很多次，刚开始我一直想在 Mac 上找软件实现此步骤，但是最终还是没找到。 vid 和 pid 搞错了，也让我浪费了很多时间。

### 安装系统

- 主机接上网线和显示器。
- 插上 U盘，选择 U盘启动。
- 当你看到显示器显示『please open http://find.synology.com ...』 这句话的时候，你就可以用另外一台电脑打开这个网页远程安装系统了。
- 安装系统的时候选择手动安装，然后选择本地之前下载好的 DSM_DS3617xs_15284.pat 文件。 此过程需要几分钟。
- 设置 QuickConnect 的时候选择跳过，因为不是真正的群晖产品，所以并不能使用此功能。

至此系统就安装完成了。最后值得注意的是不要随便的自动升级系统，可以选择自动下载系统，手动安装。

## 外网访问 NAS

### 电信宽带

如果家里使用的是电信宽带那就简单多了，直接给客服打电话说我需要一个公网 IP，一般立马就会给你了。

那么怎么查看自己是不是公网 IP呢？答：查看自己家路由器 WAN 口IP和路由器IP是否一致，一致就代表你有公网IP了。

有了公网 IP 就可以端口转发了，由于我家使用的是极路由3，自带端口转发功能（其他路由器一般都有这个功能），就可以设置把 NAS 的 IP 和端口转发到公网IP和端口。设置也非常简单，但是需要注意的是公网IP是不能使用80端口的，NAS 默认端口是 5000 你可以直接转到公网IP的5000端口。然后你试着用手机 4G 访问你的公网IP:5000 网址看看是否能成功。

IP比较难记住，你可以搞一个二级域名映射自己的公网IP。需要注意的是公网IP电信虽然给你了，但是 IP 会经常变，那么这个时候你需要一个 DDNS 服务，而正好极路由有一个[动态域名](https://app.hiwifi.com/plugin?sid=38)的插件，设置一下唯一的二级域名就可以了。

如果你不是极路由或者你想用自己的域名的话，群晖系统也支持设置 DDNS 设置：

- 把你的域名导入到 [DNSPod](https://www.dnspod.cn)，记得修改域名的 DNS。
- 进入 [DNSPod 用户中心–>安全设置–>API Token](https://www.dnspod.cn/console/user/security)，创建一个 API Token
- 然后进入群晖的 `控制面板–>外部访问–>DDNS` 按照下面这样填写信息就可以了：

```
服务提供商：选DNSPod.cn
主机名称：填要解析的域名
用户名/电子邮件：填token id
密码/密钥：填token key
```

其实原理很简单，就是通过 API，定时更新域名 IP 解析。如果是自己的域名的话还可以考虑使用 [FreeSSL](https://freessl.cn/) 申请一个免费的 HTTPS 证书玩玩。

### 其他方式

没有无法获取到公网IP的话，可以去了解一个内网穿透的一些工具，推荐使用 [frp](https://github.com/fatedier/frp/blob/master/README_zh.md)，因为最简单。

这种方式需要你自己要一个服务器。服务器上安装服务端，家里路由器安装客户端。

## 使用百度云

其实买 NAS 最主要的目的就是下载。而群晖自带的 Download Station 组件非常好用，但是却不支持下载百度云，要知道现在很多资源都是使用百度云。

如何解决这个问题呢？答案就是使用 Cloud Sync 组件，登录授权百度云账号，然后他会在 `我的应用数据` 下面创建一个 `Cloud Sync` 文件夹（**并不是同步百度云所有资料**），你可以选择双向同步或者只仅下载的方式使用。如果想下载百度云的资料就把资料复制到 `我的应用数据 -> Cloud Sync` 文件夹下面，群晖会自动下载。

省事是省事，但是速度并不是很快，只在 100kb/s 左右，有时间再看看其他方案吧。

## 客户端连接 NAS 播放视频

群晖有一个 Video Station 组件，访问一个 URL 地址可以很方便播放已下载的电影，但是在线播放的格式有限（发现不支持 MKV 格式）。解决办法就是使用客户端自带的播放器播放。

- iOS 上我推荐使用 [Infuse 5](https://3li3.com/app/view?id=185816) 通过添加『从网络分享』选择『SMB』通讯协议，免费版的基本够用。不够用的话可以花30块钱买一个 [nPlayer](https://3li3.com/app/view?id=139589)，格式支持的更多，功能更强大。
- Mac 上就简单多了，直接使用网络功能连接整个群晖的盘，像操作本地硬盘一样读取群晖里的文件。

## 最后

虽然 NAS 买了有一阵子了，但是最近太忙，没有多少时间折腾。很多东西（比方说 docker）以后有时间再玩吧，然后再来更新文章。

## 参考连接

- [黑群辉DSM 6.2.0 系统安装图文教程 (2018年08月11日更新)](https://www.nas2x.com/threads/dsm-6-2-0-20180811.29/)
- [科技產品解釋：NAS 是什麼東西呢 ?](https://www.cool3c.com/article/69281)
- [群晖（Synology）dnspod ddns使用方法](https://blog.oldghost.net:888/synology-dnspod-ddns-usage.html)
- [Synology NAS 雲端下載中心 BT 免空百度雲盤迅雷遠程一次搞懂](https://diary.taskinghouse.com/posts/synology-nas-download-center-bt-baidu-google-drive-thunder-all-in-one/)