---
title: "使用 Xdebug"
date: 2022-04-29T17:12:26+08:00
tags: ["PHP", '编程'] 
draft: false
toc: true
---

## 引言

Debug 是编程的一种能力，越早学会越好。而 Xdebug 是调试 PHP 最强的工具，本篇文章就是介绍如何使用 [Xdebug](https://xdebug.org/)。

## 安装

```shell
pecl install xdebug
```

<!--more-->

## 修改 `xdebug` 默认配置（可选）：

查看 `php.ini` 的配置文件：

```
php --ini
```

查看 debug 配置

```shell
php -i | grep xdebu
```

拿到 `php.ini` 的配置文件之后添加以下代码：

```
[xdebug]
xdebug.max_nesting_level=512
xdebug.mode=debug
xdebug.client_host=127.0.0.1
```

## 配置 Phpstorm

**PhpStorm** > **Preferences** 配置，找到 **PHP** > **Servers** ，添加一个 Server，配置如下：

- Name：`serverName`
- Host:  `127.0.0.1`
- Port: `8000`
- Debugger: `Xdebug`

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20220429mY2VXn.png)

开关 xdebug 地方在 Phpstorm 顶部的菜单，如上图所示

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202204290OS0gf.png)


开启 xdebug 之后，在需要打断点的地方前面标记一下就可以了。

## 触发
默认是不触发断点功能的

### 浏览器

安装 Chrome [Xdebug Helper extension](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc?hl=en) 扩展，会自动在每个请求里添加参数。

### Postman
在 URL 添加参数 `XDEBUG_SESSION_START=PHPSTORM`


- [Configure Xdebug](https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html)
- [Configure Xdebug for Docker](https://devdocs.magento.com/cloud/docker/docker-development-debug.html)
- [PhpStorm + Xdebug + Postman 调试环境配置](http://jalan.space/2019/04/10/2019/phpstorm-xdebug-postman/)