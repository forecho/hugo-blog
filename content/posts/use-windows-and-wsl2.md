---
title: "使用 Windows WSL2"
date: 2020-05-15T20:20:26+08:00
tags: ["经验分享", '编程'] 
draft: false
toc: true
---

## 引言

上次我的 Mac 拿去修了，换电池没那么快拿到设备，于是我又把抽屉里的公司电脑拿出来办公了，公司发的是电脑是 ThinkPad T480 笔记本，应该是定制版，没独立显卡，屏幕渣。之前安装的是 Ubuntu，感觉拿来做开发还行，除了偶尔卡住了（16G 都能卡，不知道什么问题）。另外一个最大的问题就是 Ubuntu 软件生态太少了，我比较依赖印象笔记，没有 Ubuntu 版，网页版不好用。

早就听说 Windows 现在能把 Ubuntu 当成子系统了，去查了一下资料，发现现在是 WSL2 了，虽然还在测试阶段，但是我想试一下。说了这么多本篇文章就打算分享以下两个主题：

- 如何安装 WSL2 
- 把 WSL2 作为开发环境遇到的一些坑

<!--more-->

## 准备工作

- 至少是 Window10 专业版，家庭版不行。
- 申请加入 [Windows Insider](https://insider.windows.com/zh-cn/)，然后在「Windows 设置 > 更新和安全 > Windows 预览体验计划」处选择加入快或慢。
- 在 「设置 > Windows 更新」中升级系统，通过在 `cmd` 中输入 `ver` 来确认版本为 Windows 10 18917 或更高。
- 以管理员身份运行 PowerShell 终端。
- 执行以下命令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
- 重启电脑完成安装。
- 把 WSL2 设置默认 WSL 版本的命令：

```
wsl --set-default-version 2
```
- 查看版本的命令：

```
wsl -l -v
```

## 安装 WSL2

在 「Microsoft Store」中搜索 「wsl」，点击安装「Ubuntu 18.04 LTS」和「Windows Terminal」，安装完之后我们通过 「Windows Terminal」来登录 WSL2 中的 Ubuntu 系统。

剩下的就是常规操作，换系统为国内源，安装 `ZSH` 和 `oh-my-zsh` 以及 Docker 等等。

```
cp /etc/apt/sources.list /etc/apt/sourses.list.bak
sudo vim /etc/apt/sources.list
```

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

```
sudo apt-get update
sudo apt-get upgade
sudo apt-get install zsh
# 修改默认的 Shell 为 zsh
chsh -s /bin/zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

```
# 安装软件包以允许 apt 通过 HTTPS 使用存储库
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# 设置稳定的存储库
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

# 安装最新版本的 Docker 和 containerd
sudo apt-get install docker-ce docker-ce-cli containerd.io
```


## 其他配置

### IDE 支持

VS Code 和 WSL2 配合的非常好，所以这个编辑器是必装软件。安装完之后你可以在 WSL2 Shell 里面执行 `code .` 打开此目录下的文件，首次会自动安装 [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) 扩展，非常方便。

但是如果你使用的是 JetBrains 的 IDE，想使用 WSL2 的 Shell，可以在设置里面找到 「Tools > Terminal > Shell path」 修改为：

```
"cmd.exe" /k "wsl.exe"
```

## 遇到的坑

### 动态 IP

最新版的 WSL2 已经默认帮你绑定了 localhost 到 ubuntu 系统了，所以使用 localhost （可以加端口）访问 Ubuntu 里面的服务不会有任何问题。

但是如果你想用使用自定义 nginx 的 server_name 就必须要在 Windows 里面配置 host，而对于的 `server_name` IP 并不是 `127.0.0.1` 而要在 WSL2 通过下面命令获取到对应的 IP 地址：

```sh
ip addr show eth0 | grep 'inet ' | cut -f 6 -d ' ' | cut -f 1 -d '/'
```

Github Issues 链接 <https://github.com/microsoft/WSL/issues/4210>

目前解决办法就是手动获取 IP，每次重启手动改 host。因为我很少重启电脑，所以手动还不算麻烦，如果经常操作的话，建议使用脚本。去上面的 Issues 链接找找或者看我在 V2EX 发的贴子 - [《Windows10 本地只能通过 localhost 访问 WSL2 容器？》](https://www.v2ex.com/t/671206#reply37)。

### 端口冲突问题

之前我习惯用 host 泛解析工具，Windows 下面有 [Acrylic](https://mayakron.altervista.org/support/acrylic/Home.htm) ，但是当时安装完配置好环境，重启之后发现 WSL2 不能启动了，查了一下发现 [WSL2 和 Acrylic 53 端口冲突](https://github.com/valeryan/valet-wsl/issues/14)了，所以目前是放弃使用 Acrylic 了。


### 快捷键问题

用习惯了 Mac 的快捷键就不习惯 Windows 快捷键方式了，Windows 快捷键用了两天复制粘贴按着小拇指疼，然后我就找方案。找了好几个最后找到了 [AutoHotkey ](https://www.autohotkey.com/) 这个工具，通过写代码的方式改键，可玩性非常高，配合这个人写的代码 [Elethom/ahk-mackeys](https://github.com/Elethom/ahk-mackeys) 能满足大部分需求了。

### JS 项目不能热更新

如果你的代码放在 Windows 目录下，使用 WSL2 环境，那么就会遇到代码修改之后不会热更新这个坑，使用软连接把 Windows 的目录链接到 WSL2 也不能解决。

目前唯一的解决办法就是把代码复制到 WSL2 系统里面，但是这样做意味着代码和环境绑定在一起了。系统挂了，你代码就可能找不回来了?有一定的风险。[Issues 讨论链接](https://github.com/microsoft/WSL/issues/4739)。

## 软件推荐

### [Ditto](https://ditto-cp.sourceforge.io/)

免费好用剪贴板软件

### [Listary](https://www.listary.com/)

可以理解为 Windows 下面的 Alfred

### Windows Terminal

微软出品的终端，前面已经提到过。

## 最后

微软最近这几年确实改变了很多，Windows 现在也好用多了，但是 Mac 也越来越生态化，特别是当你有一台手机和一台 Mac 电脑的时候，Mac 的[接力](https://support.apple.com/zh-cn/guide/mac-help/mchl732d3c0a/mac)让你用了就离不开了。手机和电脑粘贴板同步功能体验非常棒。

总的来说 Mac 对我来说是必备的，但是 Windows 是备胎，Windows 比较软件特别丰富（我每年抢票的软件只能在 Windows 里面运行），而且还可以玩游戏，最主要的主机便宜。

## 参考链接

- [将WSL2作为生产力工具](https://dmego.me/2019/12/21/make-wsl2-as-a-productivity-tool/)
- [在 Windows 上用 WSL 优雅开发](https://github.com/spencerwooo/dowww)