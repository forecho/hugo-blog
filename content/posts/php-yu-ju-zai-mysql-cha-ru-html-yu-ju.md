---
title: "PHP 语句在 MySQL 插入 HTML 语句"
date: 2011-10-11T10:59:00+08:00
tags: ["PHP"] 
draft: false
toc: true
---

我们需要做一个后台能手动插入 百度地图的界面。就是<http://dev.baidu.com/wiki/static/map/API/tool/creatMap/>


问题是我们要做一个给用户使用的，能自行添加的功能。 于是我们用 iframe 方法把百度这个也没调用过来了，现在关键是要把代码写入 MySQL 数据库了，我发现怎么写也写入不了。 于是在网上查了一些资料，自己尝试。发现需要转义字符串。 **与 PHP 字符串转义相关的配置和函数如下：**

```php
1.magic_quotes_runtime

2.magic_quotes_gpc

3.addslashes() 和 stripslashes()

4.mysql_escape_string()

5.addcslashes() 和 stripcslashes()

6.htmlentities() 和 html_entity_decode()

7.htmlspecialchars() 和 htmlspecialchars_decode()
```

我用的是第 3 个，OK 成功了。

其中，我们犯了一个错误，我们用 JQ 传值，把百度地图获得的代码放在 input 里面，结果 input 所获得的值是只能为一行，而我们获得的值，其中有用 “//” 注释，结果 JS 代码全给注释掉了。而且有些 JS 代码必需要换行，不然好报错。

那么发现问题的根本原因，剩下的就好解决。

花了差不多一天的时间，获得经验。