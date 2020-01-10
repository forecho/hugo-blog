---
title: "yii学习笔记（一）-yii初使用"
date: 2012-07-06T15:32:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

之前学的CI框架，现在用的已经很顺手了，感觉还不错的时候找了份工作，但是这家公司是用yii框架开发项目的，没办法，只能抓紧时间自学了。好了，废话不多说，直接进入主题。

首先刚开始学肯定是去google一下学习资料，发现了这个：<http://www.yiichina.com/> 还有这个：<http://yiiblog.info/blog/yii-go/>。这两个网站都不错。刚开始看权威指南还是有点晕晕的感觉，感觉还是上不了手，然后想到了视频，yii-go的视频还不错，很简单易懂，安装上面操作，大致了解了这个框架的用法，下面简单总结一下；

* yii框架需要执行命令启用，会自动生成一些基础的网页，功能。这个相对于ci来说，省事很多。但是刚开始我确实很不习惯。
* 简单说一下怎么生成，这个问题看了介绍的时候不是很明白，还是看视频才了解的。下载yii框架找打yiic这个文件，找到文件所属位置 ，打开cmd，然后执行 ：

```
D:
xampp\htdocs\yii\framework\yiic webapp xampp\htdocs\yiiapp
yes
```

好了，现在一个基本的web应用已经生成了，那么你现在要做的就是修改这个web为自己所用。

ps:用win+wamp的的朋友可能会遇到下面的问题：

在执行yiic webapp命令时，就报以下错误了： ‘"php.exe"’ 不是内部或外部命令，也不是可运行的程序或批处理文件。 这个事yiic批处理程序找不到php.exe引起的

1. 方法一、修改yiic.bat文件 打开yii安装目录d：/www/yii/framework/yiic.bat，yiic.bat有记事本打开有如下一行： `if "%PHP_COMMAND%" == "" set PHP_COMMAND=php.exe` 由于我的WAMP安装时没有将php.exe加入到Windows环境变量中，难怪yiic找不到。这样也只需要将php.exe的绝对路径赋给PHP_COMMAND即可。如在我这环境下是这样设置的： `if "%PHP_COMMAND%" == "" set PHP_COMMAND=D:\xampp\php\php.exe`
2. 方法二：[参考文章](http://wuhai.blog.51cto.com/2023916/760902)