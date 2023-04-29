---
title: "使用 OpenClash"
date: 2022-10-31T09:00:00+08:00
tags: ["科学上网"] 
draft: false
toc: true
---

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221102Dexz0D.png)

## 引言

最近有个需求就是想让电子书设备也能上 Google 等网站，由于电子书设备功能有限，不能安装翻墙软件，所以只能通过路由器或者软路由来实现连网科学上网。

家里的小米路由不支持翻墙插件，所以只能通过软路由来实现。正好家里有一个 NAS，可以作为软路由使用。

## OpenWrt

软路由 主要步骤就是先在我的黑群晖里面通过虚拟机功能安装一个 OpenWrt，然后在 OpenWrt 上面安装 OpenClash。

<!--more-->

跟进自己设备的架构，下载对应的系统固件，比方说我的是 x86_64 架构，所以下载的是 [x86_64 等软路由设备固件](https://openwrt.mpdn.fun:8443/?dir=lede%2Fx86_64)。不想折腾的话直接选 plus 版本的，里面已经集成了 OpenClash。


下载 `img.gz`  文件，然后解压成 `img`  文件。

### 虚拟机

在群晖上的 `Virtual Machine Manager`（VMM）里面创建一个虚拟机，步骤如下：

`映像 -> 新增 -> 从 PC 上传文件，找到之前解压的 `img`  文件 `

然后安装 OpenWrt，在 VMM 点击虚拟机，然后点击 `新增->导入`，选择 `从硬盘映像导入`，然后选择刚才的 `img`  文件，然后点击下一步：

- 给 2C CPU、2 G 内存
- 在 CPU 核心数选择的地方，右边有个小齿轮，打开，勾选开启 CPU 兼容
- 其他默认。

然后点击连接，准备修改网络（默认是 192.168.1.1）：

`vim /etc/config/network`

找到  `option ipaddr xxxx` 改成你想要的 IP 地址，比方说  `192.169.31.99`。

最后重启：

```shell
/etc/init.d/network restart
# or
reboot
```

然后访问 http://192.168.31.99/ 就可以看到 OpenWrt 的界面了，默认账号密码 `root`/`password`。

### 修改 LEDE 网络为全双工

- 去 VMM 关掉 LEDE
- 网络 - 修改网络为 `e1000` ，保存，开机

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221023l7D4KG.png)

### 软路由网络配置

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221102zBR6Ho.png)

修改网络 进入 LEDE 网络 - 接口 - 编辑 LAN：

- 添加 IPv4  网关为路由器地址，
- 在「高级设置」里面添加 DNS 服务器，比如：

```
1.1.1.1
8.8.8.8
144.144.144.144
```
- 在「DHCP 服务器」勾上「忽略此接口」

然后「保存」，再「保存并应用」

## OpenClash

我上面推荐的 OpenWrt 固件里面已经集成了 OpenClash，所以在安装好的 OpenWrt 上面的服务里面就可以找到 OpenClash 了。

> 题外话：OpenWrt 固件非常重要，我最开始使用的是一个简洁版的固件，里面要自己安装 OpenClash，然后就出现了各种问题，花了很多时间折腾最后还是没搞定，还是用了一个集成了 OpenClash 的固件，省时间。


### 确认版本

先去「全局设置」-「版本更新」页面确认配置，看看是否有红色的，最好确保所有提示都是绿色的。

### 配置 Clash 配置文件

OpenClash 有两种方式可以配置 Clash 配置文件，一种是「配置文件订阅」，一种是「配置文件管理」。看你方便，都可以。

配置好之后，点击「应用配置」，就可以了。

### 配置 OpenClash

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221102R9etfB.png)


打开 OpenClash 的「全局配置」，先配置模式，我选择的是「Fake-IP(增强）模式」，配置好之后，点击「应用配置」，就可以了。


![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221102AFhvBA.png)

「基本配置」我基本没改。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202211025Lkxko.png)

「DNS 设置」如上。其他都没怎么改。

## 如何使用

OpenClash 默认首页是「运行状态」页面，很方便看到当前的状态。使用 OpenClash 有两种方式：

### 手动模式【推荐】

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221102tnvU6d.PNG)

比较推荐这种方式，手动比较方便可靠，家里网络情况比较复杂，这种模式方便。

这种模式就是手动配置你需要走代理的的设备，一般都在 Wi-Fi 里面配置：

- 把设备的 IP 配置一个局域网段内可用的 IPv4，改最后一位数即可
- DNS 都改成软路由的地址

然后就可以了。

### 自动模式

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202211026RG3rx.png)

这种就是把软路由的 DHCP 服务器打开，并且把强制功能勾上，再把路由器的 DHCP 关闭，这样就可以让所有的设备都走代理，这种方式比较简单粗暴，但是有时候会出现一些问题。比方说我在 NAS 的几个服务就有点问题了。

还有就是我用这种方式之后手机 App 能上外网，但是浏览器就不行，不知道是什么原因。

## 最后

这个教程就到这里了，记录这些就是为了让大家少踩一些坑，节约时间，希望对你有用。

## 参考

- [OpenWrt 如何彻底禁用 IPv6 功能 - 乐猪小憩](https://keer.me/Openwrt-closes-IPv6.html)
- [在群晖上利用 VMM 玩转旁路由之 LEDE - NASDD](https://www.nasdd.cn/archives/190.html)
- [LEDE(OpenWrt) 安装 openclash ｜旁路由技巧 ｜ 配置网络流量全部通过旁路由](https://blog.frytea.com/archives/450/)