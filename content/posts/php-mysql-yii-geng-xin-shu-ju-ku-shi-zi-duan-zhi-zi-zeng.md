---
title: "PHP MySQL Yii更新数据库时 字段值自增"
date: 2013-07-30T09:30:00+08:00
categories: 
draft: false
toc: true
---

PHP MySQL的实现方法是： 
    
    
    update tablename set fieldname = fieldname*2 where ...

Yii基本上有三种实现方法： 一、用save()方法。 
    
    
    //登录一次加5点
    $user=User::model()->findByPk(Yii::app()->user->id);
    $user->reputation=$user->reputation+'5';
    $user->save(); // 将更改保存到数据库

二、用saveCounters方法。（要实现自减，只需把数值改成负数即可） 
    
    
    $postRecord=User::model()->findByPk(Yii::app()->user->id);
    $postRecord->saveCounters(array('reputation'=>5));

三、用updateCounters？这个还没有成功，报错貌似是编码问题，待解。   参考资料：<http://www.yiiframework.com/wiki/282/using-counters-with-activerecord/>