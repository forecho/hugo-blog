---
title: "Yii发邮件插件"
date: 2013-08-07T12:36:00+08:00
categories: 
draft: false
toc: true
---

一、Yii发邮件插件[下载](http://pan.baidu.com/share/link?shareid=4272079403&uk=2684558169)。 二，在配置文件/protected/config/main.php中载入组件，代码如下： 
    
    
    'components'=>array(
            'phpMailer'=>array(
                'class'=>'application.extensions.yiimailer.CPhpMailer',
                'host' => 'mail.myhost.com', //比如QQ的是smtp.qq.com
                'port' => 25,
                'from' => 'myname@myhost.com', //发件地址的用户名
                'fromName' => 'myname',//发件人
                'user' => 'username',//发件地址的用户名
                'pass' => 'password',//发件地址的密码
            ),
    ...

三、然后在控制器写入action，就可以发邮箱了，示例代码： 
    
    
    //发邮件
    public function actionSendEmail()
    {
        $mailer = Yii::app()->phpMailer->_mailer;
        $mailer->Subject = '人类已经阻止不了我发送邮件了';
        $mailer->Body = '<font color="red">hello, 我是葫芦娃</font>';
        $mailer->AddAddress('caizhenghai@gmail.com');
        //$mailer->AddAddress('xxx@gmail.com');
        $mailer->send();
    }

  参考文章：<http://www.yiichina.com/forum/topic/1417/> 其他yii发邮件插件以及方法：<http://www.yiichina.com/forum/topic/63/>