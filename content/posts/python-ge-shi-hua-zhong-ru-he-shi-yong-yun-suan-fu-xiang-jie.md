---
title: "Python格式化中如何使用%运算符详解"
date: 2013-01-10T15:04:00+08:00
categories: 
draft: false
toc: true
---

1. 如果没有什么特殊需求完全可以全部使用’%s‘来标记。
  2. 整型数：`%d`
  3. 无符号整型数：`%u`
  4. 八进制：`%o`
  5. 十六进制：`%x` `%X`
  6. 浮点数：`%f`
  7. 科学记数法: `%e` `%E`
  8. 根据数值的不同自动选择`%e`或`%f`: `%g`
  9. 根据数值的不同自动选择`%E`或`%f`: `%G`
参考： 
  1. <http://developer.51cto.com/art/201003/189039.htm>
  2. <http://woodpecker.org.cn/diveintopython/native_data_types/formatting_strings.html>