---
title: "PHP 获取当前URL的相关参数"
date: 2011-11-17T10:20:00+08:00
tags: ["PHP"] 
draft: false
toc: true
---

$_SERVER["QUERY_STRING"]获取查询语句，实例中可知，获取的是?后面的值 $_SERVER["REQUEST_URI"]

获取http://old.forecho.com后面的值，包括/

$_SERVER["SCRIPT_NAME"]

获取当前脚本的路径，如：index.php

$_SERVER["PHP_SELF"]

当前正在执行脚本的文件名

比如当前页面URL为

http://old.forecho.com/index.php?cid=1&page=4

结果如下：

```php
$_SERVER["QUERY_STRING"] = "cid=1&page=4"
$_SERVER["REQUEST_URI"] = "/index.php?cid=1&page=4"
$_SERVER["SCRIPT_NAME"] = "/index.php"
$_SERVER["PHP_SELF"]     = "/index.php"
```

另外可以参考：[PHP获取当前页面的URL](http://www.ludou.org/get_current_page_url.html#title-1)

**补充： **



```php
$_SERVER['HTTP_REFERER']
```

为获取父级页面地址，可以做返回上一级页面使用。