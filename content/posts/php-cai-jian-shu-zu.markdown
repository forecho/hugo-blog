---
title: "PHP 裁剪数组"
date: 2014-09-05T22:05:00+08:00
tags: ["PHP"] 
draft: false
toc: true
---

- 用 array_splice 可以裁剪，原来的数组会被改变，键值会重组
- 用 array_slice ，原来的数组不会改变，键值可以不用重组

示例：

``` php
echo '<pre>';
$arrayRaw = array('2' => 'c','3' => 'd', '4' => 'e', '5' => 'f');
echo "0", PHP_EOL;
print_r(array_splice($arrayRaw, 2));
echo "1", PHP_EOL;
print_r($arrayRaw);

$arrayRaw = array('2' => 'c','3' => 'd', '4' => 'e', '5' => 'f');
array_slice($arrayRaw, 0, 2, true);
echo "2", PHP_EOL;
print_r(array_slice($arrayRaw, 0, 2, true));
echo "3", PHP_EOL;
print_r($arrayRaw);
echo '</pre>';
```
<!--more-->
以上代码输出结果：

```
0
Array
(
    [0] => e
    [1] => f
)
1
Array
(
    [0] => c
    [1] => d
)
2
Array
(
    [2] => c
    [3] => d
)
3
Array
(
    [2] => c
    [3] => d
    [4] => e
    [5] => f
)
```

** 参考链接 **

[http://stackoverflow.com/questions/4804424/php-array-splice-keep-previous-keys ](http://stackoverflow.com/questions/4804424/php-array-splice-keep-previous-keys)