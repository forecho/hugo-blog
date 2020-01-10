---
title: "Yii框架中使用bootstrap"
date: 2012-12-04T15:13:00+08:00
categories: 
draft: false
toc: true
---

把下载下来的[bootstrap](http://pan.baidu.com/share/link?shareid=145927&uk=2684558169)解压之后的整个文件夹，拷贝到项目中的protected\extensions文件夹里面，并且命名为bootstrap。 修改config目录下的main.php配置文件，修改完成之后的代码如下： 
    
    
    'preload'=>array('log','bootstrap'),
    
    
    'gii'=>array(
    	'class'=>'system.gii.GiiModule',
    	'password'=>'password',
    	// If removed, Gii defaults to localhost only. Edit carefully to taste.
    	'ipFilters'=>array('127.0.0.1','::1'),
    	'generatorPaths'=>array(
    		'bootstrap.gii', // since 0.9.1
    	),
    ),
    
    
    // application components
    'components'=>array(
    	'user'=>array(
    		// enable cookie-based authentication
    		'allowAutoLogin'=>true,
    	),
    	// uncomment the following to enable URLs in path-format
    	
    	'bootstrap'=>array(
    		 'class'=>'ext.bootstrap.components.Bootstrap', // assuming you extracted bootstrap under extensions
    	 ),

使用方法参考如下：<http://www.cniska.net/yii-bootstrap/>