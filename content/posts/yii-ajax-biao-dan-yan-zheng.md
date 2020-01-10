---
title: "Yii Ajax 表单验证"
date: 2013-07-30T09:42:00+08:00
categories: 
draft: false
toc: true
---

先在Views页面的表单开启这个功能： 
    
    
    <?php $form=$this->beginWidget('CActiveForm', array(
        'id'=>'signup-form',
        'enableAjaxValidation' => true,//开启Ajax验证
        'enableClientValidation'=>true,
        'clientOptions'=>array(
            'validateOnSubmit'=>true,
        ),
    )); ?>
    
    ...
    
    <?php $this->endWidget(); ?>

然后再对应的Controller的action中加入代码： 
    
    
    //注册
    public function actionSignup()
    {   
        $model = new LoginForm('signup');
        // 开启Ajax验证
        if(isset($_POST['ajax']) && $_POST['ajax']==='signup-form')
        {
            echo CActiveForm::validate($model);
            Yii::app()->end();
        }
        if (isset($_POST['LoginForm'])) {
            $model->attributes=$_POST['LoginForm'];
            if($model->validate()){
                if($model->signup()){ 
                   $this->redirect(array('login'));
                }
            }
        }
    
        $this->render('signup', array('model'=>$model));
    }

**需要注意的是：表单的ID 名字 要跟控制器中的一样。（此处我的是`signup-form`）** ==============补充 2014年01月12日============= 如果想让 ajax 的字段唯一，只需要在 Model 文件的 Rules 方法里面加上下面这行就可以了： 
    
    
    array('name', 'unique'),