---
title: "函数式编程初探"
date: 2019-08-29T21:48:00+08:00
tags: ["分享"] 
draft: false
toc: true
---

## 引言

## 什么是函数式编程

函数式编程是是一种编程范式，它将计算机运算视为函数运算，并且避免使用程序状态以及易变对象。其中，λ演算（lambda calculus）为该语言最重要的基础。而且，λ演算的函数可以接受函数当作输入（引数）和输出（传出值）。

<!--more-->

## 入门示例

输出前 25 个整数的平方值，PHP 语言：

```php
for ($i=0; $i < 25; $i++) { 
	echo $i * $i . ',';
}
// 0,1,4,9,16,25,36,49,64,81,100,121,144,169,196,225,256,289,324,361,400,441,484,529,576,
```

如果是函数式编程语言:

```haskell
print(map (^2) [0 .. 25])
-- [0,1,4,9,16,25,36,49,64,81,100,121,144,169,196,225,256,289,324,361,400,441,484,529,576,625]
```

`print` 是一个函数，`map` 也是一个函数。

## 函数式编程有哪些特性

### 头等函数（First-class functions）

**头等函数**指的是函数与其他数据类型一样，处于平等地位，可以赋值给其他变量，也可以作为参数，传入另一个函数，或者作为别的函数的返回值。PHP 支持头等函数。围绕这一特性的应用有：

- **高阶函数（Higher-order function）**：一个函数满足至少一个参数是函数或者返回一个函数时，我们可以称这个函数为高阶函数。

高阶函数的示例一（参数是函数），返回一个数组中的偶数：

```php
$input = [1, 2, 3, 4, 5, 6];
// 匿名函数赋值给变量
$filterEven = function($item) {
    return ($item % 2) == 0;
};
$output = array_filter($input, $filterEven);
print_r($output);
// 不使用变量，直接使用闭包的版本
$output = array_filter($input, function($item) {
    return ($item % 2) == 0;
});
print_r($output);
```

高阶函数的示例二（返回函数），返回一个数组中的偶数：

```php
/**
 * 返回一个大于n的匿名函数
 */
function criteriaGreaterThan($min)
{
    return function($item) use ($min) {
        return $item > $min;
    };
}
$input = [1, 2, 3, 4, 5, 6];
$output = array_filter($input, criteriaGreaterThan(3));
print_r($output); // items > 3
```

PHP 自带的高阶函数还有 `array_map`、`array_reduce`……


- **柯里化 (Currying)**：又译为卡瑞化或加里化，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。（[来源](https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96)）

```php
// 柯里化之前
function foo($x, $y, $z) {
	echo $x + $y + $z;
}
echo foo(1, 2, 3); // 6
// 柯里化之后
function fnFoo($x, $y) {
	return function($z) use($x, $y) {
		foo($x, $y, $z);
	};
}
$bar = fnFoo(1,2);
echo $bar(3); // 6
```

使用场合（或者说是优点）：

- 参数复用
- 延迟执行

### 纯函数（Pure functions）和 不可变状态

**纯函数**

若一个函数符合以下要求，则它可能被认为是纯函数（也可以叫表达式）：

- 不依赖外部：该函数的返回结果只依赖于它的参数。
- 不改变外部：该函数不能有语义上可观察的函数副作用，诸如『触发事件』，使输出设备输出，或更改输出值以外物件的内容等。

我们来看一个例子：

```js
// 不是纯函数
const a = 1
const foo = (b) => a + b
foo(2) // => 3
// 纯函数
const a = 1
const bar = (x, b) => x + b
bar(1, 2) // => 3
```

`foo` 函数不是一个纯函数，因为它返回的结果依赖于外部变量 a，我们在不知道 a 的值的情况下，并不能保证 `foo(2)` 的返回值是 3。
`bar` 的返回结果只依赖于它的参数 x 和 b，`bar(1, 2)` 永远是 3。

纯函数的好处是没有副作用，保证了无论在什么时候调用函数，对于相同的输入，总会得到相同的输出。


**不可变状态** 

在纯函数式编程语言中，变量被赋值之后就不可以修改值了。在其他类型的语言中，变量往往用来保存一个临时状态。没有变量就不能写复杂的程序了吗？

事实上函数式程序是可以保存状态的，只不过它们用的不是变量，而是函数。状态保存在函数的参数中，也就是说在栈上。如果你需要保存一个状态一段时间并且时不时的修改它，那么你可以编写一个递归函数。举个例子：

```php
//递归实现字符串翻转
function reverseString($str)
{
	if(strlen($str) > 0) {
		reverseString(substr($str,1));
	}
	echo substr($str,0,1);
	return;
}
reverseString('forecho'); // ohcerof
```

## 尾递归优化

在函数式编程中，由于没有可变状态，for, while 这些循环都只能通过递归来实现，因此函数式编程严重依赖递归，如上面的代码示例。

我们知道递归的害处，那就是如果递归很深的话，栈受不了，并会导致性能大幅度下降。所以为了防止函数栈肆意扩展(导致栈溢出)，通常函数式语言的编译器都会实现尾调用优化。

## 对比命令式编程比较

- 命令式编程（imperative）：命令『机器』如何去做事情(how)，这样不管你想要的是什么(what)，它都会按照你的命令实现。
- 声明式编程（Declarative）：告诉『机器』你想要的是什么(what)，让机器想出如何去做(how)。

以生活中打车到王府井大街作为例子：

- 命令式编程：下个路口左转 -> 下个有红灯的路口右转 -> 前进100米 -> 在下个路口掉头 -> 前进1500米 -> 到达王府井大街出租车停车区
- 声明式编程：带我到王府井大街。

SQL 是典型的声明式编程语言，示例

```sql
SELECT * from `user` WHERE `username` = 'forecho';
```

如果用命令式编程语言改写的话：

```php
$users = ['forecho', 'cai'];
foreach ($users as $key => $user) {
	if($user == "forecho") {
     print("find");
     break;
    }
}
```

除了 SQL，网页编程中用到的 HTML 和 CSS 也都属于声明式编程，函数式编程也属于声明式编程，但是他又不仅仅局限于声明式编程。

## 函数式编程优缺点

### 优点

- 代码简洁，开发快速
- 接近自然语言，易于理解
- 更方便的代码调试与 debug：因为是纯函数
- 易于『并发编程』：无副作用
- 代码的热升级

### 缺点

- 执行效率并不高，性能比命令式编程差
- 函数式编程用类似管道的方式来处理数据，因此不适合处理可变状态。
- 函数式编程不适合做 IO 操作，也不适合写 GUI。

## 最后

除了本篇文章介绍的函数式编程，其他常见编程范式都有响应式编程、命令式编程、面向对象编程、结构式编程等等。

需要说明的是在整理这篇文章的时候其实我本人并没有函数式编程的经验，所以这里只做了初步分享，等以后我有了经验再来分享。

## 参考链接

- [函数式编程](https://en.wikipedia.org/wiki/Functional_programming)
- [函数式编程](https://coolshell.cn/articles/10822.html)
- [λ演算](https://zh.wikipedia.org/wiki/%CE%9B%E6%BC%94%E7%AE%97)
- [Functional Programming in PHP](https://phptherightway.com/pages/Functional-Programming.html)
- [大佬，JavaScript 柯里化，了解一下？](https://juejin.im/post/5af13664f265da0ba266efcf)
- [理解函数式编程](https://wudaijun.com/2018/05/understand-functional-programing/)