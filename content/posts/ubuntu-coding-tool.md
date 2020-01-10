---
title: "Ubuntu 开发利器"
date: 2018-09-27T22:02:00+08:00
tags: ["编程工具", "Ubuntu"] 
draft: false
toc: true
---

## 前言

三年前我写了一篇文章 - [《Windows 开发利器》](https://blog.forecho.com/windows-coding-tool.html)，开头吐槽了 Linux，之后推荐了一些 Windows 开发利器。由于上周我换了一份工作，公司开发人员除特殊情况外，其他大部分人都统一使用 Ubuntu 系统。经过一个多星期的吐槽和适应，虽然没有 Mac 系统的做开发爽，但也还算满意。

这篇文章我将介绍我使用 Ubuntu 开发时候用到的一些软件和经验。

<!--more-->

## 经验

### 快捷键

推荐复制、粘贴使用以下快捷键

- `Ctrl + Insert` 复制
- `Shift + Insert` 粘贴


### 安装 deb 软件包

能看到安装软件需要的一些依赖，命令：

```sh
sudo dpkg -i package.deb
```

### 强制退出软件的命令

示例：强制退出 datagrip 

```bash
ps aux | grep -i datagrip | grep -iv grep | awk '{print $2}' | xargs kill -s 9
```


## 软件

### 输入法：[搜狗](https://pinyin.sogou.com/linux/?r=pinyin)

### 剪贴板软件：[Diodon](https://launchpad.net/diodon)

安装：

```bash
sudo add-apt-repository ppa:diodon-team/stable
sudo apt-get update
sudo apt-get install diodon
```

使用：

系统-》设置-》键盘-》键盘快捷键-》添加

名称：Diodon 剪贴板  
命令：`/usr/bin/diodon`  
快捷键：`Ctrl + Shift + v`


### Git 客户端 

- [Gitkraken](https://www.gitkraken.com/)
- [Sublime Merge](https://www.sublimemerge.com/)：Sublime 团队最新出品

### 终端：[Terminator](https://launchpad.net/terminator)

```sh
sudo apt-get install terminator
mkdir ~/.config/terminator && vim ~/.config/terminator/config
```

```
[global_config]
  handle_size = -3
  enabled_plugins = CustomCommandsMenu, LaunchpadCodeURLHandler, APTURLHandler, LaunchpadBugURLHandler
  title_transmit_fg_color = "#000000"
  suppress_multiple_term_dialog = True
  title_transmit_bg_color = "#3e3838"
  inactive_color_offset = 1.0
[keybindings]
[profiles]
  [[default]]
    palette = "#000000:#5a8e1c:#2d5f5f:#cdcd00:#1e90ff:#cd00cd:#00cdcd:#e5e5e5:#4c4c4c:#868e09:#00ff00:#ffff00:#4682b4:#ff00ff:#00ffff:#ffffff"
    background_image = ""
    background_darkness = 0.92
    scrollback_lines = 3000
    background_type = transparent
    use_system_font = False
    scroll_background = False
    show_titlebar = False
    cursor_shape = ibeam
    font = Liberation Mono 12
    background_color = "#0e2424"
    foreground_color = "#e8e8e8"
[layouts]
  [[default]]
    [[[child1]]]
      type = Terminal
      parent = window0
      profile = default
    [[[window0]]]
      type = Window
      parent = ""
      size = 925, 570
[plugins]
```

使用：

```
Ctrl+Shift+E    垂直分割窗口
Ctrl+Shift+O    水平分割窗口
Ctrl+Shift+W    关闭分割的窗口
F11             全屏
Ctrl+Shift+C    复制
Ctrl+Shift+V    粘贴
Ctrl+Shift+N    或者 Ctrl+Tab 在分割的各窗口之间切换
Ctrl+Shift+X    将分割的某一个窗口放大至全屏使用
Ctrl+Shift+Z    从放大至全屏的某一窗口回到多窗格界面
Ctrl+Shift+T    新建标签页
Ctrl+Shift+Q    退出程序
Ctrl+Shift+PageUp/PageDown 切换标签  
```

### 窗口管理：[gTile](https://github.com/gTile/gTile)

安装：

```sh
git clone https://github.com/gTile/gTile.git ~/.local/share/gnome-shell/extensions/gTile@vibou
```

```
Alt-F2
Enter a Command: r
```

使用：

```
Win+←         靠左二分之一
Win+→         靠右二分之一
Win+↓         居中
Win+↑         全屏
Win+Shift+←   去左边屏幕（多屏）
Win+Shift+→   去右边屏幕（多屏）
```

### 快速启动器：[Cerebro](https://cerebroapp.com/)

Alfred for Ubuntu，默认启动 `ctrl+空格键`

位置不能移动的问题，解决办法：修改 `sudo vim ~/.config/Cerebro/config.json` 的 `positions` 值，参考[连接](https://github.com/KELiON/cerebro/issues/315)。

### 印象笔记：[nixnote2](https://github.com/baumgarr/nixnote2)

```sh
sudo add-apt-repository ppa:nixnote/nixnote2-daily
sudo apt update
sudo apt install nixnote2
```

> 在登录国服时候，输入完邮箱地址一直不会显示密码框，这个时候点击左上角的“印象笔记”链接，然后会打开网页版的印象笔记页面，在里面找到登录页面，正常登录。然后关闭登录框，再从”工具“中的”同步“进入，这个时候就会看到授权提示了。

### QQ

[2018年wine QQ最完美解决方案（多Linux发行版通过测试并稳定运行）](https://www.lulinux.com/archives/1319)

### 编辑器

- [Sublime Text](http://www.sublimetext.com/)
- [VS Code](https://code.visualstudio.com/)
- [PhpStorm](https://www.jetbrains.com/phpstorm/)


### 下载软件 - [uGet+aria2](https://www.cnblogs.com/EasonJim/p/7119294.html)

安装 

```sh
sudo add-apt-repository ppa:plushuang-tw/uget-stable
sudo add-apt-repository ppa:t-tujikawa/ppa
sudo apt-get update
sudo apt-get install uget
sudo apt-get install aria2
```


### 截图软件 - [Flameshot](https://github.com/lupoDharkael/flameshot)

安装

点击 [releases](https://github.com/lupoDharkael/flameshot/releases) 页面下载最新版本的代码，然后双击安装即可。

使用

只需要配置快捷键就可以了，在 `设置-键盘` 中，将 `flameshot gui` 命令绑定到 `Ctrl+Alt+A` 即可（根据个人习惯）。功能很强大，使用习惯跟 QQ 截图一样。

### 代码比对软件 - [Meld](http://meldmerge.org/)

安装 

```sh
sudo apt-get install meld
```

使用

直接去应用程序里面搜索 `meld` 或者在命令行输入：

```sh
meld&
```

### 音乐 - [网易云音乐](https://music.163.com/#/download)

我的系统是 Ubuntu 18.04，官网下载最新1.1版本安装打不开，网上找的各种解决办法麻烦而且不太好使。但是可以下载 [1.0 版本](http://mirrors.ustc.edu.cn/debiancn/pool/main/n/netease-cloud-music/netease-cloud-music_1.0.0%2Brepack.debiancn-1_i386.deb)安装，安装命令：

```
sudo dpkg -i netease-cloud-music_1.0.0+repack.debiancn-1_i386.deb
sudo apt-get -f install
sudo dpkg -i netease-cloud-music_1.0.0+repack.debiancn-1_i386.deb
```

第二条和第三条命令主要是解决依赖问题。

## 最后

Ubuntu 作为软件开发系统，除了软件比较少之外，其实还挺不错的。Windows 和 Mac 系统常用得软件在 Ubuntu 都能找到代替品，可以将就着用，唯一让我不太满意的就是剪贴板软件了，Diodon 不太稳定，找机会再换一个。

三大系统，让我选我还是选 Mac，作为开发和美观都非常满意。Ubuntu 和 Windows 只能说各有优缺点。