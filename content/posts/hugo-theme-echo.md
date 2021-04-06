---
title: "Hugo 极简主题 Echo"
date: 2021-04-06T12:33:00+08:00
tags: ["Hugo"] 
draft: false
toc: true
---

## 引言

去年年初的时候博客从 [Octopress 迁移到 Hugo](https://blog.forecho.com/octopress-migrate-to-hugo.html)，那个时候找遍了很多 Hugo 主题都不满意，于是自己写了一个。

这就是 [echo](https://github.com/forecho/hugo-theme-echo) 主题的由来。最近又优化了一下这个主题，就顺便写篇文章给大家介绍一下这个主题，刚写出来的时候只在 [V2EX 上](https://www.v2ex.com/t/637170#reply13)分享过。

## 特色

### 风格基于 [Tailwind CSS](https://tailwindcss.com/)

TailwindCSS 可以说是这两年比较火的 CSS 框架了，看腻了 [Bootstrap](https://getbootstrap.com/) 的样式，觉得 TailwindCSS 真漂亮，但是在使用上不同于 Bootstrap， TailwindCSS 给人的一种感觉是面向过程写代码，可以自己自由组装各种样式，缺点是学习门槛高一点。而且可能要写大量重复的代码。

这次采用 TailwindCSS 做为主题 CSS 框架，一是因为它样式好看，二是尝鲜，换思路写 CSS。

<!--more-->

### 深色模式

1.0 版本的 TailwindCSS 还不支持深色模式，现在最新版 2.0 了，所以我也就升级了，顺便支持了深色模式，为了支持深色模式，代码结构改动很多，不得不引入了一些其他工具（比如说 Nodejs、PostCSS 等），其实我很不愿意把一个主题项目搞得那么复杂，但是尝试了一番之后，最后发现不得不这样做。

这次加入深色模式，工作量比预期的要多，

### 那年今日

我非常喜欢 Google 相册以及 Day One 的「那年今日」功能，所以这次我在「[文章归档](https://blog.forecho.com/posts.html)」中也实现了此功能，我认为此功能：

- 可以帮助自己回忆过往写过的博客
- 也可以激励自己多多写博客

### 热门文章

通过给文章添加「popular」标签，可以实现热门文章，在右侧和 404 页面都有展示热门文章列表。

### 更加友好的 SEO

在 SEO 上也做了很多优化，文章如果被分享到 Twitter/Facebook 上会有卡片效果。


### 其他基本功能

- 自定义 CSS，自定义 JS
- 文章支持目录
- 支持相关阅读
- 使用更快的 Chroma 代码高亮

## 最后

- 主题的地址 [hugo-theme-echo](https://github.com/forecho/hugo-theme-echo)
- [本博客的源码地址](https://github.com/forecho/hugo-blog)：你可以参考我的 Hugo 配置信息和 GitHub Actions 配置