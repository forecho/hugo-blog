---
title: "简单介绍 Rails 默认文件结构"
date: 2013-12-09T09:54:00+08:00
categories: 
draft: false
toc: true
---

文件/文件夹 说明

`app/`
程序的核心文件，包含模型、视图、控制器和帮助方法

`app/assets`
程序的资源文件，如 CSS、JavaScript 和图片

`bin/`
可执行文件

`config/`
程序的设置

`db/`
数据库文件

`doc/`
程序的文档

`lib/`
代码库文件

`lib/assets`
代码库包含的资源文件，如 CSS、JavaScript 和 图片

`log/`
程序的日志文件

`public/`
公共（例如浏览器）可访问的数据，如出错页面

`script/rails`
生成代码、打开终端会话或开启本地服务器的脚本

`test/`
程序的测试文件（在 [3.1.2 节](http://railstutorial-china.org/chapter3.html#section-3-2-1) 中换用 `spec/`）

`tmp/`
临时文件

`vendor/`
第三方代码，如插件和 gem

`vendor/assets`
第三方代码包含的资源文件，如 CSS、JavaScript 和图片

`README.rdoc`
程序简介

`Rakefile`
`rake` 命令包含的任务

`Gemfile`
该程序所需的 gem

`Gemfile.lock`
一个 gem 的列表，确保本程序的复制版使用相同版本的 gem

`config.ru`
[Rack 中间件](http://rack.rubyforge.org/doc/)的配置文件

`.gitignore`
git 忽略的文件类型
来源：<http://railstutorial-china.org/chapter1.html> 参考资料：<http://ruby-china.org/topics/2432>