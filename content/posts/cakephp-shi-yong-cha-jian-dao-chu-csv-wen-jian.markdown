---
title: "CakePHP 使用插件导出 CSV 文件"
date: 2014-09-05T21:42:00+08:00
tags: ["CakePHP"] 
draft: false
toc: true
---

插件地址：[https://github.com/josegonzalez/cakephp-csvview](https://github.com/josegonzalez/cakephp-csvview)

用法就不说了，上面写的很详细了，补充两点吧。

**1. 是数字显示的问题**

有时候数字很大，导出的文件打开，数字变成科学计数法显示了，这显然不是我们想要的，手动在每个值后面添加一个制表符「\t」就解决这个问题了。代码如下：

``` php
foreach ($data as $key => $value) {
    $item[$key]['username'] = $value['username']. "\t";
    $item[$key]['mobile']   = $value['mobile'] . "\t";
    $item[$key]['created']  = $value['created'] . "\t";
}
```
<!--more-->
**2. 导出文件 office 中文乱码**

Plugin/CsvView/View/CsvView.php 文件的 _renderRow 函数，大概在 251 行之后添加如下代码解决中文乱码：

``` php
if ($row) {
    foreach ($rowas$key => $value) {
        $row[$key] = iconv('utf-8','gb2312',$value);
    }
}
```