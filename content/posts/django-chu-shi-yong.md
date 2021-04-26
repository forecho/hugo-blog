---
title: "Django初使用"
date: 2013-04-26T23:36:00+08:00
categories: 
draft: false
toc: true
---

Python的项目可以放在你电脑的任何一个文件夹中。 转到你创建的目录，运行命令`django-admin.py startproject mysite`。这样会在你的当前目录下创建一个目录。（名为`mysite`的项目）

### 文件如下：
 
 - `__init__.py` ：让 Python 把该目录当成一个开发包 (即一组模块)所需的文件。 这是一个空文件，一般你不需要修改它。 
 - `manage.py` ：一种命令行工具，允许你以多种方式与该 Django 项目进行交互。 键入python manage.py help，看一下它能做什么。 你应当不需要编辑这个文件；在这个目录下生成它纯是为了方便。 
 - `settings.py` ：该 Django 项目的设置或配置。 查看并理解这个文件中可用的设置类型及其默认值。 
 - `urls.py`：Django项目的URL设置。 可视其为你的django网站的目录。 目前，它是空的。   
 
 
 ### 启动服务器
 
 切换到项目目录里 (cd mysite )，运行下面的命令： `python manage.py runserver` 于是就会返回一个项目的浏览地址（默认一般是：http://127.0.0.1:8000）。 
 
 退出这个状态可以使用：CTRL+C。 默认情况下， runserver 命令在 8000 端口启动开发服务器，且仅监听本地连接。 
 
 要想要更改服务器端口的话，可将端口作为命令行参数传入： `python manage.py runserver 8080` 通过指定一个 IP 地址，你可以告诉服务器–允许非本地连接访问。 
 
 如果你想和其他开发人员共享同一开发站点的话，该功能特别有用。 ` 0.0.0.0` 这个 IP 地址，告诉服务器去侦听任意的网络接口。（这个IP必须是本机的IP地址） `python manage.py runserver 0.0.0.0:8000` 
 
 注：三引号，是python换行字符串的格式。

## Comments

**[baocaixiong](#146 "2013-05-01 22:00:57"):** 这个主题。。我想要。。。

