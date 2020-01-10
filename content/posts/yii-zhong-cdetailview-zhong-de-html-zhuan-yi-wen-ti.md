---
title: "Yii中CDetailView中的HTML转义问题"
date: 2013-07-30T09:44:00+08:00
categories: 
draft: false
toc: true
---

<?php $this->widget('zii.widgets.CDetailView', array(
        'data'=>$model,
        'attributes'=>array(
            'about',
            array(
                'name'=>t('about', 'model'),
                'type'=>'ntext',
                'value'=>$model->about,
            ),
        ),
    )); ?>

我这里只是显示textarea里面的</br>标签。 所以type选的是ntext。   参考资料： <http://www.yiichina.com/api/CDetailView#attributes-detail> <http://www.yiichina.com/api/CFormatter>