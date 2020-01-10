---
title: "解决 Yii 1.12 中 Session 丢失的问题"
date: 2013-05-28T20:48:00+08:00
categories: 
draft: false
toc: true
---

_（这篇文章是转载的，原文排版有问题，网页也错乱了，没办法我只有转载这篇文章了，[来源](http://www.woqilin.net/2012/12/yii-112-session.html)。）_ Session 丢失的问题在运用一些框架来开发比较常见。原因是你对框架 Session 的机制不是很了解，它有可能运用了自己的一套机制。 情况一般是用 header 跳转后才发生的，因为在 header 之前还能够成功打印出来。 一般 Session 跨页面丢失的问题有以下几种情况： 1.客户端禁用 Cookie 。因为 Session 默认是基于 Cookie 的，因为找不到相应的 Cookie，所以 Session 会为空。 2.你在给 Session 赋值时，前面已经调用了 session_write_close() 函数。在原页面虽然能够成功打印，但是并没有成功保存 Session。 而这2中情况在 Yii 都不是，打开文件 yii\framework\web\auth\CWebUser.php #700 
    
    
    Yii::app()->getSession()->regenerateID(true);

把这个语句注释掉就可以了。它默认调用了PHP 自带的一个函数 
    
    
    bool session_regenerate_id ( [bool delete_old_session] )

按常理来说是不会出现 Session 丢失问题的。这可能和我的配置或者它内部实现 Seesion 的机制有关系。 你还可以查看PHP 手册 《Session 会话处理函数》 一章，了解更多详情。