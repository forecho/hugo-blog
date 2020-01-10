---
title: "substr(),mb_substr()及mb_strcut的区别和用法"
date: 2012-11-01T13:59:00+08:00
tags: ["php"] 
draft: false
toc: true
---

substr()函数可以分割文字，但要分割的文字如果包括中文字符往往会遇到问题，这时可以用mb_substr()/mb_strcut这个函数，mb_substr()/mb_strcut的用法与substr()相似，只是在mb_substr()/mb_strcut最后要加入多一个参数，以设定字符串的编码，但是一般的服务器都没打开php_mbstring.dll，需要在php.ini在把php_mbstring.dll打开。 举个例子： 

```php
<?php
    echo mb_substr('这样一来我的字符串就不会有乱码^_^', 0, 7, 'utf-8');
?>
```

输出：这样一来我的字

```php
<?php
    echo mb_strcut('这样一来我的字符串就不会有乱码^_^', 0, 7, 'utf-8');
?>
```

输出：这样一

从上面的例子可以看出，mb_substr

是按字来切分字符，而mb_strcut是按字节来切分字符，但是都不会产生半个字符的现象……mbstring

函数的说明：

php的mbstring扩展模块提供了多字节字符的处理能力，平常最常用的就是用mbstring来切分多字节的中文字符，这样可以避免出现半个字符的情况，由于是php的扩展，它的性能也要比一些自定义的多字节切分函数要好上一些。

mbstring extension提供了几个功能类似的函数，mb_substr和mb_strcut，看看手册上对它们的解释。 mb_substr mb_substr() returns the portion of str specified by the start and length parameters. mb_substr() performs multi-byte safe substr() operation based on number of characters. Position is counted from the beginning of str. First character's position is 0. Second character position is 1, and so on. mb_strcut mb_strcut() returns the portion of str specified by the start and length parameters. mb_strcut() performs equivalent operation as mb_substr() with different method. If start position is multi-byte character's second byte or larger, it starts from first byte of multi-byte character. It subtracts string from str that is shorter than length AND character that is not part of multi-byte string or not being middle of shift sequence. 

再举个例子,有一段文字, 分别用mb_substr和mb_strcut来做切分: 

```php
<?php
    $str = '我是一串比较长的中文-www.webjx.com';
    echo "mb_substr:" . mb_substr($str, 0, 6, 'utf-8');
    echo "<br>";
    echo "mb_strcut:" . mb_strcut($str, 0, 6, 'utf-8');
?>
```
输出结果如下：

```
    mb_substr:我是一串比较
    mb_strcut:我是
```