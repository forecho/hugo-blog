---
title: "PHP 中的动态属性和 StdClass"
date: 2015-11-08T21:52:00+08:00
tags: ["php", "翻译"] 
draft: false
toc: true
---

> 翻译 [Dynamic Properties in PHP and StdClass](http://krisjordan.com/dynamic-properties-in-php-with-stdclass)

JavaScript 和 Python 允许对象实例的动态属性。事实证明,PHP 也是如此。看官方 PHP 对象和类文档可能会导致你相信动态实例属性需要自定义 __get 和 __set 魔术方法。其实不用。

## 简单,内置的动态属性

看看以下代码:

```php
class DynamicProperties { }
$object = new DynamicProperties;
echo isset($object->foo) ? 't' : 'f'; // f
echo PHP_EOL;
// Set Dynamic Properties foo and fooz
$object->foo = 'bar';
$object->fooz = 'baz';
// Isset and Unset work
isset($object->foo); // true
unset($object->foo);
// Iterate through Properties and Values
foreach($object as $property => $value)  {
    echo $property . ' = ' . $value . PHP_EOL;
}
// Prints:
// fooz = baz
```

<!--more-->

使用内置动态实例属性的速度比使用魔法 __get 和 __set 方法快一个数量级（30倍,通过我的分析）。在 PHP 中动态属性访问时是没有回调方法。

因此,什么情况下需要使用 __get 和 __set 呢？如果你需要更复杂的行为,比如需要计算属性,你必须使用__get和__set。另外，如果你不愿意让类拥有动态属性你可以使用__get和__set抛出错误。

## StdClass: Anonymous Objects

有时一个属性包扔到键值对是必要的。一种方法是使用数组,但这需要引用所有的键。另一种方法是使用动态属性 StdClass 的实例。StdClass 是 PHP 中没有预定义的成员属性的特殊类。

```
$object = new StdClass;
$object->foo = 'bar';
json_encode($object);
```

接下来我使用 SPL's 的 Countable 和 ArrayAccess 可以实现下面的几个动作:

```
class MyClass implements Countable, ArrayAccess { ... }
$myObject = new MyClass();
// Using array access notation
$myObject[0] = 'hello';
$myObject[1] = 'world';
$myObject['foo'] = 'bar';
```

实现动态属性功能并不需要继承 stdClass！