---
title: "CI 框架学习笔记（一）"
date: 2011-11-07T14:54:00+08:00
tags: ["CodeIgniter"] 
draft: false
toc: true
---

* **模型 (Model)**代表你的数据结构。通常来说，你的模型类将包含取出、插入、更新你的数据库资料这些功能。 （操作数据库）
* **视图 (View)**是展示给用户的信息。一个视图通常是一个网页，但是在 CodeIgniter 中，一个视图也可以是一个页面片段，如页头、页尾。它还可以是一个 RSS 页面，或任何其它类型的“页面”。（前台页面）
* **控制器 (Controller)**是模型、视图以及其他任何处理 HTTP 请求所必须的资源之间的_中介_，并生成网页。 （链接两者）
1. 模型文档名字如果与数据库重名则不能使用常规方法使用写。
2. 控制器里面，一个方法代表一个页面。
3. 自动加载数据库：在 autoload.php 中写入代码（42 行）

```php
$aotoload['libraries']=array('database');
```

4. 设定相对路径前面的前缀：（放在 header.php 文件 title 下面）

```php
<base href="<?php echo base_url();?>"/>
```

5. 获取变量：（3 代表 index.php 之后的第三个参数）

```
$this->uri->segment(3)
```

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160745.png)

6. config.php 下的代码（266 行）(如果为 true 则开启：自动防止攻击。)

```
$config['global_xss_filtering'] = FALSE
```
