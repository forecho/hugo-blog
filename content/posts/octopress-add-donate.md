---
title: "Octopress 添加打赏"
date: 2016-01-28T15:36:00+08:00
tags: ["Octopress"] 
draft: false
toc: true
---

## 前言

本来今天是打算给博客添加一个打赏功能的，但是搜索了一下没发现 Octopress 有这个插件，然后就不知道怎么得就折腾 CC 协议插件了。

在折腾的过程中，我突然就学会写插件了，其实很简单。然后我就搜索到一个 hexo 打赏插件（因为懒得写样式和布局），经过几次修改，Octopress 打赏插件就这样诞生了。

## 如何添加

<!--more-->

主要分四步：

- 添加 `source/_includes/post/donate.html` 文件，内容可以使用我的 [donate.html](https://github.com/forecho/blog/blob/master/source%2F_includes%2Fpost%2Fdonate.html)
- 修改 `sass/custom/_styles.scss` 样式文件，添加[样式代码](https://github.com/forecho/blog/blob/master/source%2F_includes%2Fpost%2Fdonate.html)，样式代码有点挫，是因为我不会 scss。
- 修改 `source/_layouts/post.html` 布局文件，添加[代码](https://github.com/forecho/blog/blob/master/source%2F_layouts%2Fpost.html#L9-L11)。
- 修改 `_config.yml` 配置文件，添加[代码](https://github.com/forecho/blog/blob/master/_config.yml#L138-L140)。

以上操作完成就可以了。

