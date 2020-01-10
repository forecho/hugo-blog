---
title: "Yii一些用法总结"
date: 2013-01-09T14:14:00+08:00
categories: 
draft: false
toc: true
---

Yii在View 获取action的代码： 
    
    
    strtolower($this->action->id);//转换成小写

Yii 在main.php配置文件中添加如下代码，可以实现如下功能： 
    
    
    'timeZone' => 'Asia/Shanghai', //解决时差问题
    'language' => 'zh_CN', //开启中文提示
    'defaultController'=>'options/welcome',//默认控制器、方法

在其中db数组中添加下面这行代码可实现 数据库表前缀功能： 
    
    
    'tablePrefix' => 'yii_',  //表前缀