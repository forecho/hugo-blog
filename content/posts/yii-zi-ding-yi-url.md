---
title: "yii自定义URL"
date: 2012-11-05T16:20:00+08:00
categories: 
draft: false
toc: true
---

1、首先找到 protected/config/main.php配置文件。找到如下图代码块： ![](http://m3.img.libdd.com/farm4/2012/1105/15/CF0E6785535F6CD9AECFC4A40FE5A827A793CD4B4B31D_672_337.PNG) 把urlManager整个的注释都取消掉。 2、添加两行代码，示例如下： 
    
    
    'urlManager'=>array(
                'urlFormat'=>'path',
                'showScriptName' => false,// 使用URL重写，去掉index.php 
                'urlSuffix' => '.html',//开启伪静态
                'rules'=>array(
                    '<controller:\w+>/<id:\d+>'=>'<controller>/view',
                    '<controller:\w+>/<action:\w+>/<id:\d+>'=>'<controller>/<action>',
                    '<controller:\w+>/<action:\w+>'=>'<controller>/<action>',
                ),
            ),

3、需要在项目的更目录下创建.htaccess内容如下： 
    
    
    Options +FollowSymLinks
    IndexIgnore */*
    RewriteEngine on
    
    # if a directory or a file exists, use it directly
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    
    # otherwise forward it to index.php
    RewriteRule . index.php

OK，这时候基本的需求已经满足了，下面我们要扩展一下 **urlManager** 有时候会根据项目的需求需要扩展url，那么这个时候我们只需要简单的在urlManager的rules里面扩展就OK了。示例代码如下： 
    
    
    array(
        'posts'=>'post/list',
        'post/<id:\d+>'=>'post/read',
        'post/<year:\d{4}>/<title>'=>'post/read',
    )

  * 调用$this->createUrl('post/list')生成/index.php/posts。第一个规则适用。
  * 调用$this->createUrl('post/read',array('id'=>100))生成/index.php/post/100。第二个规则适用。
  * 调用$this->createUrl('post/read',array('year'=>2008,'title'=>'a sample post'))生成/index.php/post/2008/a%20sample%20post。第三个规则适用。
  * 调用$this->createUrl('post/read')产生/index.php/post/read。请注意，没有规则适用。