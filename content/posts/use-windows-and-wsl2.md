---
title: "使用 Windows WSL2"
date: 2020-05-15T20:20:26+08:00
tags: ["经验分享", '编程'] 
draft: true
toc: true
---

## 引言

上次我的 Mac 拿去修了，换电池没那么快拿到设备，于是我又把抽屉里的公司电脑拿出来办公了，公司发的是电脑是 ThinkPad T480 笔记本，应该是定制版，没独立显卡，屏幕渣。之前安装的是 Ubuntu，感觉拿来做开发还行，除了偶尔卡住了（16G 都能卡，不知道什么问题）。另外一个最大的问题就是 Ubuntu 软件生态太少了，我比较依赖印象笔记，没有 Ubuntu 版，网页版不好用。

早就听说 Windows 现在能把 Ubuntu 当成子系统了，去查了一下资料，发现现在是 WSL2 了，虽然还在测试阶段，但是我想试一下。说了这么多本篇文章就打算分享以下两个主题：

- 如何安装 WSL2 
- 把 WSL2 作为开发环境遇到的一些坑

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

目前解决办法就是手动获取 IP，每次重启手动改 host

## 最后
