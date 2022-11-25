---
title: "我是如何减少我的鼠标使用的？"
date: 2022-11-25T15:58:00+08:00
tags: ["经验分享"] 
draft: false
toc: true
---

## 引言

虽然鼠标是一个伟大的发明，但是作为一种非常方便的输入设备，它也有一些缺点，比如说：

- 没有键盘效率高
- 会让你得[「鼠标手」](https://zh.wikipedia.org/wiki/%E9%BC%A0%E6%A0%87%E6%89%8B)

所以我觉得还是日常使用电脑工作较多的职业还是多用键盘比较好。

<!--more-->

## 如何减少鼠标使用？

### 给常用应用设置启动快捷键

多个软件我们来回切换，可以用 `Ctrl（Command） + Tab` 来切换，但是这个切换效率不高。

我目前使用的是 [Hammerspoon](https://www.hammerspoon.org/) 来设置快捷键来切换应用，比如我设置了 `Alt + g` 来切换到 Chrome，`Alt + c` 来切换到 VSCode，`Alt + i` 来切换到 iTerm2，这样就可以很方便的切换应用了。

Hammerspoon 功能强大，免费，但是上手有一点门槛，你可以通过看[Mac 利器 Hammerspoon 使用指南](https://blog.forecho.com/use-hammerspoon.html#app-%E5%90%AF%E5%8A%A8) 文章和参考[我的配置](https://github.com/forecho/hammerspoon-config)来完成此项功能。如果你实在搞不定 Hammerspoon，可以考虑使用 [Alfred](https://www.alfredapp.com/) 来实现此功能。

另外补充：

- 不常用的软件可以使用 [Alfred](https://www.alfredapp.com/) 或者[Raycast](https://raycast.com/) 来快速启动
- 一个应用有多个窗口时，可以使用 `Ctrl（Command） + ~` 来切换窗口

### 使用 Vim

写代码的时候使用 Vim 模式，现在很多编辑器都支持 Vim 模式，比如 VSCode，Sublime Text，WebStorm 等等。按照对应的插件即可使用 Vim 模式。

效率不是一般的高，关于 Vim 的使用可以参考我的[重学 Vim](https://blog.forecho.com/re-study-vim.html) 这篇文章。

### Google Chrome

浏览器现在已经是我们最常用的软件之一了，不管你会不会 Vim 你都可以通过安装 [vimium-c](https://github.com/gdh1995/vimium-c) 扩展程序来使用 Vim 模式来浏览网页。

如果你不会 Vim，那么你看一下他的使用快捷键说明也可以很方便的使用。

最常用的就是：

- `f` 打开链接
- `u` 后退
- `d` 前进
- `gg` 回到顶部
- `G` 回到底部
- `x` 关闭标签页
- `r` 刷新
- `gi` 聚焦第一个输入框

不常用的：

- `o` 打开新标签页
- `O` 打开新窗口
- `yy` 复制当前链接
- `j` 向下滚动
- `k` 向上滚动
- `R` 强制刷新

另外补充 Chrome 常用快捷键：

- `Ctrl（Command）+ t` 打开新标签页
- `Ctrl（Command）+ w` 关闭标签页
- `Ctrl（Command）+ y` 显示历史记录
- `Ctrl（Command）+ Shift + t` 恢复关闭的标签页
- `Ctrl（Command）+ Shift + n` 打开隐身模式
- `Ctrl（Command）+ Shift + j` 打开下载页面
- `Ctrl（Command）+ Alt + i` 打开开发者工具

### Mac

补充一个 Mac 常用的快捷键：

- `Command + control + q` 锁屏，再按 `ESC` 就立即睡眠

## 最后

虽然我们离不开鼠标，但是我们可以改变自己的使用习惯，减少鼠标的使用，提高效率。

以上就是我目前正在使用可以减少使用鼠标的一些技巧，如果你有更好的技巧欢迎在评论区留言。


