---
title: "yii怎样修改自动跳转到登录页面？"
date: 2012-12-05T17:35:00+08:00
categories: 
draft: false
toc: true
---

**1、在配置文件main.php下的components里添加下面代码：**
    
    
    'user'=>array(
    	// enable cookie-based authentication
    	'allowAutoLogin'=>true,
    ),

**2、在SiteController.php添加如下代码：**
    
    
    public function filters()
    {
    	return array(
    		'accessControl', // perform access control for CRUD operations
    	);
    }
    public function accessRules()
    {
    	return array(
    		array('allow',  //未登录用户允许操作的action
    				'actions'=>array('login','logout','register'),
    				'users'=>array('*'),
    		),
    		array('allow',   //登录用户允许操作全部action
    				'users'=>array('@')
    		),
    		array('deny',  // allow all users to perform 'index' and 'view' actions
    				'users'=>array('*'),
    		),
       );
    }

**更新一下更完善的方法：** **2、在SiteController.php添加如下代码：**
    
    
    public function accessRules()
    {
    	return array(
    		array('allow',  // allow all users to perform 'index' and 'view' actions
    			'actions'=>array('login','error'),
    			'users'=>array('*'),
    		),
    		array('allow', // allow admin user to perform 'admin' and 'delete' actions
    			'actions'=>array('logout'),
    			'users'=>array('@'),
    		),
    		array('deny',  // deny all users
    			'users'=>array('*'),
    		),
    	);
    }

**3、在配置文件main.php下添加下面代码：**
    
    
    'defaultController'=>'options/welcome',//默认加载的控制器 页面

**4、在OptionsController.php修改accessRules，修改后的代码如下：**
    
    
    public function accessRules()
    {
    	return array(
    		array('allow', // allow admin user to perform 'admin' and 'delete' actions
    			'actions'=>array('welcome'),
    			'users'=>array('admin'),
    		),
    		array('deny',  // deny all users
    			'users'=>array('*'),
    		),
    	);
    }