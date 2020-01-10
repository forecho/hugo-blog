---
title: "yii隐藏index.php而不隐藏admin.php的方法"
date: 2012-11-06T10:47:00+08:00
categories: 
draft: false
toc: true
---

之前我介绍过[yii模块的使用](/archives/515)，使用模块的方法可以帮我们实现前后台分离的工作，但是本人不是很喜欢这个方法，个人认为模块是用来扩展的。 于是我又使用单独的入口文件，具体可以参考[这篇文章。](http://www.yiichina.com/forum/thread-54-1-1.html) 这个时候我又想隐藏掉前台URL中的index.php，于是在网上找了一些资料，整合了[这篇文章](http://old.forecho.com/archives/702)。 但是现在问题出现了，安装那个办法确实把前台中的index.php隐藏掉了，但同时也隐藏掉了admin.php，于是进入后台的时候URL就乱套了。路径都有问题。 **解决办法就是：** 在protected/admin/config/main.php文件中加入下面一行代码： 
    
    
    $frontendArray=require($frontend.'/config/main.php');
    unset($frontendArray['components']['urlManager']);//不隐藏后台URL中的admin.php