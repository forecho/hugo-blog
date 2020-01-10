---
title: "Yii扩展自带的时间选择器插件CJuiDatePicker"
date: 2014-01-13T16:48:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

事情大概是这样的：我数据库时间字段用的是 int 类型，存储的是时间戳。并且我写文章的时候，时间是需要可以修改的。

来一张最终效果图： ![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160820.png)

首先我用了 Yii 自带的 Zii CJuiDatePicker 扩展，非常的强大的把 jQuery UI 的 [Datepicker](http://jqueryui.com/datepicker/) 继承进去了，使用非常的简单，手册上有。但是这个有个缺陷，没有小时分钟，只能配置日期。这个有点坑爹了。然后没办法，去找了一个 yii 的扩展。我选了 [ejuidatetimepicker](http://www.yiiframework.com/extension/ejuidatetimepicker/)，使用也很简单，基本就按照给的 Demo复制过去就可以了。

现在来说说这个功能的难点，首先我数据库是 int 时间戳格式的，但是我用的这个插件是日期格式的，那么现在我又不想改数据库时间的字段类型，我要如何解决？ Google 了一下，找到了一篇文章，我按照这个来结果成功了。

首先根据插件的 Demo 我们在视图form的时间是这样写的：


    <?php echo $form->labelEx($model,'create_time'); ?>
        <?php $this->widget('application.extensions.timepicker.EJuiDateTimePicker',array(
            'model'=>$model,
            'attribute'=>'create_time',
            'language'=>'zh-CN',
            'options'=>array(
                'hourGrid' => 4,
                'hourMin' => 9,
                'hourMax' => 17,
                'timeFormat' => 'hh:mm',
                'changeMonth' => true,
                'changeYear' => false,
                ),
            'htmlOptions'=>array(
                'readonly'=>true,
                'style'=>'width:180px;'
            ),
        )); ?>
    <?php echo $form->error($model,'create_time'); ?>

控制器部分我们基本上不用修改。接下来我们去修改 Model 文件，添加两个 yii 的 function：


    protected function beforeSave()
    {
        $this->admin_id = Yii::app()->user->id;
        // $this->create_time = date('Y-m-d', CDateTimeParser::parse($this->create_time, 'yyyy-MM-dd hh:mm'));
        $this->create_time = strtotime($this->create_time);
        return parent::beforeSave();
    }

    protected function afterFind()
    {
        $this->create_time = Yii::app()->dateFormatter->format('yyyy-MM-dd hh:mm', $this->create_time);
        return parent::afterFind();
    }

`beforeSave` 就是数据保存之前我们要处理的事件。`afterFind` 就是数据显示之前我们要处理的事件。 非常巧妙的用法。

最后别忘记了把 Model 的 rules 的时间验证规则改一下，代码如下：


    array('create_time', 'date', 'format'=>'yyyy-MM-dd hh:mm', 'message'=>'{attribute} have wrong format'),

**值得注意的是 rules 验证会在 beforeSave 之前执行。beforeSave 和 afterFind 都是 protected 属性。

** 参考文章：**
- <http://www.yiiframework.com/extension/ejuidatetimepicker/>
- <http://www.yiiframework.com/doc/api/1.1/CJuiDatePicker>
- [http://www.volkomenjuist.nl/blog/2013/02/24/yii-cjuidatepicker-dateformat/ ](http://www.volkomenjuist.nl/blog/2013/02/24/yii-cjuidatepicker-dateformat/)  

最后感叹下自带 zii 的方便强大，再分享一个资源： <http://www.bsourcecode.com/yii-framework/yii-extensions/>

## Comments

**[Jack Van](#206 "2014-08-22 12:05:00"):** 怎么只能从9点到16点

**[ForEcho](#208 "2014-10-19 15:41:00"):** 代码上面有参数。自己可以设置，hourMin 和 hourMax

