---
title: "Yii Select下拉菜单"
date: 2013-07-30T09:38:00+08:00
categories: 
draft: false
toc: true
---

Yii给下拉菜单一个添加一条数据： 一、不需要默认值 
    
    
    <?php echo $form->dropdownlist(
                        $model,
                        'parent_id',
                        CHtml::listData(Cat::model()->findAll(), 'id', 'name'),
                        array('prompt'=>'根目录')
    ); ?>

'prompt'可以换成'empty' 结果是： 
    
    
    <select name="Cat[parent_id]" id="cat_parent_id">
        <option value="">根目录</option>
        <option value="1">手机</option>
        <option value="2">婚纱</option>
    </select>

  二、如果自己的添加的一个Option需要给定Value的话，只能用‘empty’来实现，示例如下： 
    
    
    <?php echo $form->dropdownlist(
                        $model,
                        'parent_id',
                        CHtml::listData(Cat::model()->findAll(), 'id', 'name'),
                        array('empty'=>array('10'=>'根目录'))
    ); ?>

结果是： 
    
    
    <select name="Cat[parent_id]" id="cat_parent_id">
        <option value="10">根目录</option>
        <option value="1">手机</option>
        <option value="2">婚纱</option>
    </select>

  参考资料： <http://www.yiichina.com/api/CHtml#activeDropDownList-detail> <http://stackoverflow.com/questions/16057637/yii-dropdown-list-empty-value-as-default>