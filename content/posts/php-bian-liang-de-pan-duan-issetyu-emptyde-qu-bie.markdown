---
title: "PHP - isset 与 empty 的区别"
date: 2014-09-05T21:18:00+08:00
tags: ["PHP"] 
draft: false
toc: true
---

**当一个变量没有声明的时候，第一个 `if` 会报错**

``` php
if ($a) {
   echo $a.'1'.PHP_EOL;
}
if (!empty($a)) {
   echo $a.'2'.PHP_EOL;
}
if (isset($a)) {
   echo $a.'3'.PHP_EOL;
}
```
<!--more-->
**当一个变量声明了，并且值为空的时候，`isset` 会通过，此处输出结果是 a3**

``` php
$a='';
if ($a) {
   echo $a.'1'.PHP_EOL;
}
if (!empty($a)) {
   echo $a.'2'.PHP_EOL;
}
if (isset($a)) {
   echo $a.'3'.PHP_EOL;
}
```

**当一个变量声明了，并且有值的时候，三个都会通过，此处输出结果是 a1 a2 a3**

``` php
$a='a';
if ($a) {
   echo $a.'1'.PHP_EOL;
}
if (!empty($a)) {
   echo $a.'2'.PHP_EOL;
}
if (isset($a)) {
   echo $a.'3'.PHP_EOL;
}
```

##总结：
1. 结论就是从数据库查询出结果的时候，如果要做判断就直接做判断或者用 `empty` 去做判断，如果用 `isset` 去做判断就毫无意义。
2. `empty` 可以判断变量是否存在并且值是否为空，所以尽量用 `empty` 做判断。
3. `isset` 只能判断变量是否声明。

