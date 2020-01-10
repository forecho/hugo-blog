---
title: "Yii调试错误以及一些技巧（持续更新）"
date: 2013-04-02T10:37:00+08:00
categories: 
draft: false
toc: true
---

**Yii添加不了数据：**
    
    
    $admin = new Admin;        
    $admin->username = $username; 
    $admin->password = $password; 
    if($admin->save()>0){ 
       echo "添加成功";  
    }else{  
       echo "添加失败";  
    }

上面是一条最简单的Yii添加数据方法，按着这个来写，能读出数据，但是还是无法写入数据库，利用下面的这段代码我们可以找出错误的原因。 
    
    
    $admin->save();
    var_dump($admin->errors);
    exit;

最后一般能找出原因。 **Yii的findAll打印输出问题：**
    
    
    $project=Project::model()->findAll();
    foreach($project as $v){
        echo $v->attributes['title'];
    }

**findAll条件查询简写：**
    
    
    $criteria = new CDbCriteria(array(
        'condition' => '(id=35 OR id=36) AND commend=2 AND status=2',//多个条件查询
        'limit' => '1',
        'order'=>'id DESC',
        //'order'=>'RAND()',  //随机筛选
     ));
     $video = News::model()->findAll($criteria);

或者可以这样写： 
    
    
    $video = News::model()->findAll(array(
        'condition'=>'(id=35 OR id=36) AND commend=2 AND status=2', 
        'order'=>'id DESC', 
        'limit'=>1,
    ));

**Yii页面包含其它页面方法：**
    
    
    <?php $this->renderPartial('/comment/_form',array(
     'model'=>$comment,
     )); ?>

**清空缓存**
    
    
    Setting::model()->destructCache();

**更新数据**
    
    
    //第一个是根据条件更新多行数据
    //第二个是根据主键更新单条数据
    Posts::model()->updateAll(array('title'=>'Hello World'), array('condition'=>'user='.Yii::app()->user->getId()));
    $count = User::model()->updateByPk($_POST['userid'], array('name'=>'forecho'));
    if($count>0)
    {
    echo "更新成功";
    }

**关于分页联表查找时，字段重复的问题**
    
    
    $dataProvider=new CActiveDataProvider('Post', array(
        'criteria'=>array(
            'condition'=>'status=1',
            'order'=>'create_time DESC',
            'with'=>array('author'),//调用relations
        ),
        'pagination'=>array(
            'pageSize'=>20,
        ),
    ));

主表用的是主表名，关联表是关系名. 读取数据的时候，比如说两个表ID字段重复了。要读取第二个ID的话，可以这样读取: 
    
    
    $data->author->id//根据文章userid关联user表读取id

**添加JS和CSS**
    
    
    Yii::app()->clientScript->registerCoreScript(Yii::app()->homeUrl.'/js/jquery.js');
    Yii::app()->clientScript->registerCssFile(Yii::app()->homeUrl.'/css/style.css');

**验证规则添加情景（页面）** Yii的验证规则要写在Model的rules里面，例如下面的这行代码： 
    
    
    array('psword, email, old_psword, re_psword, captchal', 'required', 'on'=>'forgot'),

解释一下，有时候不止一个页面会使用同一个Model里面的验证规则，这个时候为了不冲突，我们需要设置情景，上面的`'on'=>'forgot'`就是自定义的情景。如果要使用的话，还需要要在相应的控制器的action里面添加情景，代码如下： 
    
    
    $model->scenario = 'forgot'; //设置当前情况下的验证场景

**Yii里设置SESSION过期时间** 在app config里，设置： 
    
    
    'components'=>array(
        'session'=>array(
            'timeout'=>3600,
        ),
    )

**查询个数**
    
    
    $count = Notification::model()->countByAttributes(array(
                'user_id'=> Yii::app()->user->uid
            ));

**Yii 数据save后得到插入id**
    
    
    $model->save();
    //得到上次插入的Insert id
    $id = $model->attributes['id'];

**添加手机号码验证** 在Model里面的rules添加代码： 
    
    
    array('phone', 'match', 'pattern'=>'^13[0-9]{9}|15[012356789][0-9]{8}|18[0256789][0-9]{8}|147[0-9]{8}$','message'=>'请输入正确的手机号码'),