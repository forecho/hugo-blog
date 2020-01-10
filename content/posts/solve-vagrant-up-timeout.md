---
title: "解决「vagrant up」timeout 问题"
date: 2015-07-30T22:38:00+08:00
tags: ["Vagrant", "VirtualBox", "吐槽"] 
---
## 前言

之前我有写[《Windows 开发利器》](/blog/windows-coding-tool.html#section-1)文章提到使用「VirtualBox + Vagrant（打造 Linux 开发环境）」，
到现在为止在公司使用也有几个月了，到现在为只出现过两次问题。

## 第一次启动不了

第一次主要是我电脑下班之后没关机，睡眠模式，结果半夜停电了，导致强制关机了，最后导致 `vagrant up` 命令启动不了了，最后我又重新安装了我的 vagrant。
所以，切记，**一定要学会打包自己的 vagrant**。

**打包自己的 vagrant**

```sh
$ vagrant package
[default] Attempting graceful shutdown of VM...
[default] Clearing any previously set forwarded ports...
[default] Creating temporary directory for export...
[default] Exporting VM...
[default] Compressing package to: /Users/astaxie/vagrant/package.box
```

有了这个备份就是可以放心的使（zhe）用（teng）vagrant 了。

下面我们重点来讲讲今天遇到的这个坑。

<!--more-->

## 第二次启动不了

今天我跟往常一样在终端用`vagrant up`命令去启动的时候，一直显示的是「timeout」启动不了。然后我打开 VirtualBox，使用界面尝试着去启动，但是刚开机 boot 完之后就提示我大概意思是：系统不支持64位，CPU 不支持虚拟化。
真是日了狗了，昨天之前都是用的好好的，突然就说不支持了，之前有搜过资料知道要改 BIOS 设置，但问题是我之前都是好好的，解释不通啊。

然后我以为是我的软件有问题，下载了好几个版本的 VirtualBox，发现 VirtualBox 5 都出来了，但是安装之后 vagrant 说不支持，然后又去更新 vagrant，真是各种折腾。

VirtualBox 有的版本启动系统的时候，报错，最后上网搜索，发现 4.3.12 版本的可以正常使用，安装后真的可以使用。

但是最后还是因为「系统不支持64位，CPU 不支持虚拟化」还是不行，好吧，我要去 BIOS 看看到底是什么个情况，哪知道我那电脑又是映泰主板的，真是坑，找半天最后才百度知道在什么鬼地方的。为什么要用缩写写的方式，真是坑爹。

找到之后发现是「enabled」状态，当时我就懵逼了，设置没问题啊。好吧，我又去开启折腾 VirtualBox 去了，又去换版本常识，最后都快有放弃使用 vagrant 的念头了。

最后一生气，又跑去 BIOS 改虚拟化参数，我关闭再试试，最后果然还是不行。

好吧，重启，我再进 BIOS 改虚拟化参数，我开启再试试，没想到这次成功了。我擦，我心里面一万个草泥马在奔跑啊，Windows 啊！

看就是这货：

![Imgur](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424161033.png)

中间不知道重启了多少次电脑，浪费了多少时间，项目进度都被耽误了，而且为这破事我还加班了。

不知道是不是昨天公司停电导致的，但是我记得我有关电脑啊。

![你他妈逗我吗](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424161011.png)


----------------draft: false
toc: true
---

**参考文章**：

- [路径（七）：用 Vagrant 管理虚拟机](http://ninghao.net/blog/2077)
- [Vgrant使用入门](https://github.com/astaxie/Go-in-Action/blob/master/ebook/zh/01.3.md)
- [映泰A3怎么开启虚拟化](http://zhidao.baidu.com/link?url=HxDvjGXnQCVV73G1MtkZyIO4ym3nbSMScrLfsAnjsPIXakl39kuCtGuUtzGkVoEbPT2fBlhsi5X-iQPhF7eoBa)