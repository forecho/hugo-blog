---
title: "Octopress 添加 CC 协议"
date: 2016-01-28T13:08:00+08:00
tags: ["Octopress"] 
draft: false
toc: true
---

## 前言

如果你不知道什么是 CC 协议，或者你不知道选什么协议，请访问[Choose a Licens](http://creativecommons.org/choose/)（内容是中文的）。

## 如何添加

我基本上是使用插件 [hoatle/octopress-cc-license](https://github.com/hoatle/octopress-cc-license) 完成的此功能，但是稍作修改，汉化了内容。

<!--more-->

主要分四步：

- 添加 `source/_includes/post/cc_license.html` 文件，内容可以使用我的 [cc_license.html](https://github.com/forecho/blog/blob/master/source%2F_includes%2Fpost%2Fcc_license.html)
- 修改 `sass/custom/_styles.scss` 样式文件，添加[样式代码](https://github.com/forecho/blog/blob/master/sass%2Fcustom%2F_styles.scss#L22-L38)。
- 修改 `source/_layouts/post.html` 布局文件，添加[代码](https://github.com/forecho/blog/blob/master/source%2F_layouts%2Fpost.html#L12-L14)。
- 修改 `_config.yml` 配置文件，添加[代码](https://github.com/forecho/blog/blob/master/_config.yml#L118-L136)。

以上操作完成就可以了。

