---
title: "Yii关于登录表单汉化操作"
date: 2012-12-06T10:27:00+08:00
categories: 
draft: false
toc: true
---

Yii自动生成的登录页面已经很完善了，非常好用，但是唯一不足的就是界面是英文版的，对于我们来说非常不友好，用户可能根本看不懂。 改为中文版的方法其实很简单，找到`models/LoginForm.php`模型文件，找到如下代码： 
    
    
    public function attributeLabels()
    {
    	return array(
    		'rememberMe'=>'Remember me next time',
    	);
    }

**改为如下代码：**
    
    
    public function attributeLabels()
    {
    	return array(
    		'username'=>'用户名',
    		'password'=>'密　码',
    		'rememberMe'=>'记住并自动登录',
    	);
    }

OK,现在你去刷新你的登录页面，就会是中文的了，你不必去改动login.php页面的任何代码。