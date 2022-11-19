---
title: "电子书繁体竖排转换简体横排"
date: 2020-03-19T20:57:28+08:00
tags: ["工具", "kindle"] 
draft: false
toc: true
---

## 引言

我现在有一本繁体竖排的电子书，竖排真的是没那么阅读习惯。繁体看着也累，于是想到了做个转换，于是网上搜索资料，但搜索到的都是与我反过来的，他们要把简体横排转换成繁体竖排。

于是我只能自己摸索着如何把『电子书繁体竖排转换简体横排』，然后整理了成这篇文章。

<!--more-->

## 工具

如果你只需要把横版繁体转换成横版简体，你可以试试[天火藏書](http://ebook.cdict.info)，在线就能搞定，简单快捷。（竖排的电子书虽然给转成横排了，但是标点符号有问题）

如果你是竖排的电子书，那就接着往下看吧。准备工具：

- 下载最新版 [calibre](https://calibre-ebook.com/download)
- 安装 calibre 的『Chinese Text Conversion』插件，搜索「Chinese」即可：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221119sAQYl7.png!m)

## 步骤

- 导入电子书到 calibre
- 然后编辑对应的书籍
- 然后在菜单栏找到『插件』然后点击我们刚安装的『Chinese Text Conversion』插件，设置参数，然后点确定就可以了。参考设置如下图（注意根据自己需求修改设置）：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/k1QTP2.jpg!m)


保存之后点「阅读」就可以看效果了。如果繁体转简体成功，但是排版还是竖版的话，需要继续编辑书，改动改一下代码。

需要检查 css 文件，如果找到 `vertical-rl` 改为 `horizontal-tb` 即可。改完保存退出编辑模式，使用阅读预览效果。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20221119iJhYls.png!m)

## 最后

网上搜索了很多资料说要修改代码（修改 opf 和 css 文件），实测最新版的『Chinese Text Conversion』插件已经不需要那么做了，给开发者点赞。

## 资料

- [電子書直橫轉換有什麼困難？](https://bobtung.medium.com/%E9%9B%BB%E5%AD%90%E6%9B%B8%E7%9B%B4%E6%A9%AB%E8%BD%89%E6%8F%9B%E6%9C%89%E4%BB%80%E9%BA%BC%E5%9B%B0%E9%9B%A3-5926fa019003)