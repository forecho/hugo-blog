---
title: "PHP 最高效的去判断数组最后一个或第一个元素"
date: 2013-04-09T17:21:00+08:00
tags: ["PHP 基础"] 
draft: false
toc: true
---

首页这个方法我是无意中在知乎上看到的，确实很实用，[原文地址～](http://www.zhihu.com/question/20158667/answer/15243506) 

思路：先把数组中的第一个元素或最后一个元素用 PHP 自带的函数删掉，然后单独使用的时候可以单独调用。 

删掉数组**最后一个**元素用[array_pop()](http://www.w3school.com.cn/php/func_array_pop.asp)这个函数。 示例： 

```php
<?php
    $a=array("Dog","Cat","Horse");
    print_r(array_pop($a));
    echo '</br>';
    print_r($a);
?>
```
输出结果：

```
Horse
Array ( [0] => Dog [1] => Cat )
```

删掉数组**第一个**元素用[array_shift()](http://www.w3school.com.cn/php/func_array_shift.asp)这个函数。方法跟上面类似，我就不写示例来。 
这个方法效率很高，很实用。非常感谢原作者。  

**值得注意的地方：**

下面我们来说说使用这个[array_shift()](http://www.w3school.com.cn/php/func_array_shift.asp)函数，值得注意的地方，在下面[这个网址](http://writecodeonline.com/php/)输入以下代码： 

```php
$a=array('3' => "Dog", 'a' => "Cat", '0' => "Pig", '4' => "Horse");
// print_r(array_pop($a));
print_r(array_shift($a));
echo '</br>';
print_r($a);
```
那么输出的结果会是这个：

```
Dog
Array ( [a] => Cat [0] => Pig [1] => Horse )
```
是不是与想象中不一样，数组中的最后一个值的键位由 4 变成了 1，所以使用的时候要注意。但是[array_pop()](http://www.w3school.com.cn/php/func_array_pop.asp)这个函数没有出现这种情况。