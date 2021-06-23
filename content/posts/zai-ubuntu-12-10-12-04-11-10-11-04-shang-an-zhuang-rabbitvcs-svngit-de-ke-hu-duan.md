---
title: "在Ubuntu(12.10/12.04/11.10/11.04)上安装RabbitVCS SVN，Git的客户端"
date: 2013-06-22T15:26:00+08:00
categories: 
draft: false
toc: true
---

1. 首先安装一些环境：

```sh
$ sudo apt-get update
$ sudo apt-get install python-nautilus python-configobj python-gtk2 python-glade2 python-svn python-dbus python-dulwich subversion meld gconf-editor
```

2. 将压缩包下载，解压并运行安装脚本：

```sh
$ wget http://rabbitvcs.googlecode.com/files/rabbitvcs-0.15.2.tar.bz2
$ tar jxvf rabbitvcs-0.15.2.tar.bz2
$ cd rabbitvcs-0.15.2/
$ sudo python setup.py install --install-layout=deb
```

cd 到 nautilus-3.0 的客户端目录下和复制的 RabbitVCS.py 扩展目录：

```sh
$ cd clients/nautilus-3.0
$ sudo cp RabbitVCS.py /usr/share/nautilus-python/extensions/
```

注销并重新登录。现在创建一个文件夹，右键点击它，看看 RabbitVCS 菜单。

- 原英文地址：<http://linuxdrops.com/install-rabbitvcs-svn-git-client-on-ubuntu-12-1012-0411-1011-04/>
- apt-get 自动安装：<http://www.webupd8.org/2011/01/rabbitvcs-perfect-tortoisesvn.html>
