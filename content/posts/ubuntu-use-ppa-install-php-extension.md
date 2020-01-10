---
title: "Ubuntu 使用 PPA 安装 PHP 扩展"
date: 2015-11-19T10:57:00+08:00
tags: ["php"] 
draft: false
toc: true
---

## 简介

PPA，即 Personal Package Archives 缩写，也就是个人软件包集。

> 有很多软件因为种种原因，不能进入官方的 Ubuntu 软件仓库。 为了方便 Ubuntu 用户使用，launchpad.net 提供了 ppa，允许用户建立自己的软件仓库， 自由的上传软件。PPA 也被用来对一些打算进入 Ubuntu 官方仓库的软件，或者某些软件的新版本进行测试。

## 使用

我一直不太喜欢用源码编译的方式安装 PHP 扩展，主要是麻烦。然后偶然发现了这种方式安装 PHP 扩展，真是太方便了。

下面我们来讲讲怎么使用：

安装 PPA

```
sudo add-apt-repository ppa:ondrej/php5
sudo apt-get update
```

安装 php mcrypt 扩展

```
sudo apt-get install php5-mcrypt
```

<!--more-->

安装 php imagemagick 扩展

```
sudo apt-get install imagemagick
sudo apt-get install php5-imagick
sudo php5enmod mcrypt
```

安装 php gd 扩展

```
sudo apt-get install php5-gd
```

安装 php apc 扩展

```
sudo apt-get install php-apc
```

安装 php intl 扩展

```
sudo apt-get install php5-intl
```

**切记：安装扩展完要重启 PHP 才能看效果**

```
sudo service php5-fpm reload
```

PPA 这么强大当然也可以安装 PHP

```
sudo apt-get install php5-fpm
```

## 参考文章

- [软件包管理](http://people.ubuntu.com/~happyaron/udc-cn/lucid-html/ch11s02.html)
- [ubuntu通过ppa源安装php5.4或php5.5](http://blog.x228.com/archives/316.html)