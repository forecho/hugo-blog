---
title: "yii学习笔记（二）-修改配置文件"
date: 2012-07-09T16:08:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

配置文件在 protected\config\main.php **开启gii** 找到下面这段代码，把注释去掉：

```php
/*
'gii'=>array(
    'class'=>'system.gii.GiiModule',
    'password'=>'Enter Your Password Here',
    // If removed, Gii defaults to localhost only. Edit carefully to taste.
    'ipFilters'=>array('127.0.0.1','::1'),
),
*/
```

修改password为自己使用的密码。 ps：如果访问gii的时候提示文件不存在，那就修改ipFilters中127.0.0.1为自己的IP。 **开启数据库** 找到下面这段代码：

```php
'db'=>array(
    'connectionString' => 'sqlite:'.dirname(__FILE__).'/../data/testdrive.db',
),
// uncomment the following to use a MySQL database
/*
'db'=>array(
    'connectionString' => 'mysql:host=localhost;dbname=testdrive',
    'emulatePrepare' => true,
    'username' => 'root',
    'password' => '',
    'charset' => 'utf8',
),
*/
```

把上面一段注释掉，下面一段开启，修改之后如下：

```php
/*
'db'=>array(
    'connectionString' => 'sqlite:'.dirname(__FILE__).'/../data/testdrive.db',
),
*/
// uncomment the following to use a MySQL database

'db'=>array(
    'connectionString' => 'mysql:host=localhost;dbname=appyii',
    'emulatePrepare' => true,
    'username' => 'root',
    'password' => '',
    'charset' => 'utf8',
    'tablePrefix'=>'fe_',//指表前缀，需要的自行添加
),
```
dbname:指数据库名。