---
title: "Yii关于messages，多语言切换"
date: 2012-12-05T11:37:00+08:00
categories: 
draft: false
toc: true
---

**1\. 设置全局默认的语言** 之前提到过在protected/config/main.php 文件内添加如下代码，默认语言为中文（有限的为中文） 
    
    
    'language' => 'zh_cn',

**2.控制器根据用户选择动态切换语言**
    
    
    public function init()
    {
        if(isset($_GET['lang']) && $_GET['lang'] != "")
        {
            Yii::app()->language = $_GET['lang'];
            Yii::app()->request->cookies['lang'] = new CHttpCookie('lang', $_GET['lang']);
        }
        else if(!empty(Yii::app()->request->cookies['lang']))
        {
            Yii::app()->language = Yii::app()->request->cookies['lang'];
        }
        else
        {
            $lang = explode(',',$_SERVER['HTTP_ACCEPT_LANGUAGE']);
            Yii::app()->language = strtolower(str_replace('-', '_', $lang[0]));
        }
    }

**3\. 页面提供切换语言选项** 在公用的 layouts 头部，加入 
    
    
    <?php echo CHtml::link('中文', Yii::app()->createUrl('/', array('lang' => 'zh_cn')));?>
    <?php echo CHtml::link('English', Yii::app()->createUrl('/', array('lang' => 'en_us')));?>

**4\. 多语言描述文字**
    
    
    //admin是对应的语言文件，路径：protected/messages/zh_cn/admin.php
    Yii::t('admin', 'Management Center');

zh_cn/admin.php代码如下： 
    
    
    <?php
    return array(
        'Management Center' => '管理中心',
    );