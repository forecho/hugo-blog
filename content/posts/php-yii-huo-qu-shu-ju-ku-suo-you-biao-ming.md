---
title: "PHP Yii 获取数据库所有表名"
date: 2013-08-02T21:15:00+08:00
categories: 
draft: false
toc: true
---

再次感叹Google和English的强大，所以以后找问题一定要在Google加英语的关键词。 MySQL获取所有的表名很简单，就一行代码如下： 
    
    
    SHOW TABLES

Yii获取所有的表名，代码如下： 
    
    
    foreach(Yii::app()->db->schema->getTables() as $name=>$table)
    {
    	echo $table->name;
    }

  参考文章： <http://blog.sjzycxx.cn/post/328/> <http://www.farifam.com/site/content/365-Show+All+Table+Database+In+Yii+Framework.html>