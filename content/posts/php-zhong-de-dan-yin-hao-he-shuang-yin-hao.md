---
title: "PHP中的单引号和双引号"
date: 2013-05-18T14:53:00+08:00
tags: ["PHP基础"] 
draft: false
toc: true
---

被双引号的内容能被PHP检测其中的变量，并且自动转换变量，示例：

```
<?php
	$name = 'forecho';
	echo 'Hello world $name';
	echo '</br>';
	echo "Hello world $name";
?>
```

输出的结果就是：

```
Hello world $name
Hello world forecho
```
这样就可以得出结论，其实单引号更快，因为单引号不用去考虑变量问题，而是直接输出。

更多资料，请参考鸟哥的[这篇文章](http://www.laruence.com/2008/08/19/338.html)。但是写HTML的时候，写页面一些属性的时候，W3C标准，还是要用双引号。