---
title: "yii学习笔记（五）-修改新建Create页面"
date: 2012-07-12T15:47:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

我们可以用gii生成views里面的页面，很方便的，但是，并不是所有的东西都是我们所需要的，所以我们要学会修改代码，为我们所用。 **改为下拉选项框** 添加某个字段的时候，把手动输入的input表单改成下拉选项框。 首页，我们要在字段对应的表的m下写入一个方法。比方说我现在一个type_id字段对应的是news_type表。 那么我们在m文件夹中找到NewsType.php文件，在末尾加上这段代码：


    // 获取type
    public function getNewsTypeList()
    {
        $newsTypeList = NewsType::model()->findAll();
        return CHtml::listData($newsTypeList,'id','news_type_name');

        //下面是用来验证的代码
        // $returnData = CHtml::listData($newsTypeList,'id','news_type_name');
        // print_r($returnData);
        // exit;
    }

然后我们找到Create所在的页面，这里是Admin/views/news/_form.php文件。 找到这段代码：


    <?php echo $form->textField($model,'type_id',array('size'=>10,'maxlength'=>10)); ?>

修改为：


    <?php echo $form->dropDownList($model,'type_id',NewsType::model()->getNewsTypeList()); ?>

刷新一下就会得到我们想要的下拉选项效果。 **系统默认添加的信息** 那么有些情况下，我们会遇到一个字段是由系统自动帮我们添加的，比方说，添加新闻的人，时间。 我们先把_form.php文件里字段相应的代码删掉。 然后去找这个表的m文件，这里是News.php文件 在末尾加上这段代码：


    //自动添加新闻时间、状态
    protected function beforeSave()
    {
        if (parent::beforeSave()) {
            if ($this->isNewRecord) {

            //如果是新的新闻

                $this->create_time = date('Y-m-d H:i:s');
                $this->create_user_id = Yii::app()->user->id;
            }else {
                $this->update_time = date('Y-m-d H:i:s');
                $this->update_user_id = Yii::app()->user->id;
            }
                return true;
            }else {
                return false;
        }

    }

**读取数据的时候转换** 其实这个时候数据记录type_id字段的任然是数字，那么读取的时候我们还需要转换一下。 在读取页面的m对应的文件中修改代码，这里对应的是News.php文件 我们找到下面这段代码：


    public function relations()
    {
        // NOTE: you may need to adjust the relation name and the related
        // class name for the relations automatically generated below.
        return array();
    }

添加修改为：


    public function relations()
    {
        // NOTE: you may need to adjust the relation name and the related
        // class name for the relations automatically generated below.
        //表链接
        return array(
            'typeName'=>array(self::BELONGS_TO,'NewsType','type_id'),
            'statusName'=>array(self::BELONGS_TO,'StatusType','status_id'),
        );
    }

然后去找到要显示的页面，这里是Admin/views/news/_view.php 找到这段代码：


    <?php echo CHtml::encode($data->type_id); ?>

修改为：


    <?php echo CHtml::encode($data->typeName->news_type_name); ?>

statusName也是这样修改。

## Comments

**[桂林老医](#111 "2012-07-13 16:04:30"):** :shock: 看不懂
