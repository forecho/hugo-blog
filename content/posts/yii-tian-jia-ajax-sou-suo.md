---
title: "Yii添加Ajax搜索"
date: 2013-06-08T14:04:00+08:00
categories: 
draft: false
toc: true
---

其实默认就有，只需要开启就好了。 首先先把列表写好。列表用`'dataProvider'=>$product->search(40),//数据源 `调用数据源 **View页面：** 添加Ajax： 
    
    
    <?php
    Yii::app()->clientScript->registerScript('search', "
    $('.search-form form').submit(function(){
    	$.fn.yiiGridView.update('product-grid', {
    		data: $(this).serialize()
    	});
    	return false;
    });
    ");
    ?>

表单： 
    
    
    <div class="pro_filter c_f search-form">
    <?php $form=$this->beginWidget('CActiveForm', array(
    	'action'=>Yii::app()->createUrl($this->route),
    	'method'=>'get',
    )); ?>
    <label>产品标题：</label>
    <?php echo $form->textField($product,'name',array('class'=>'t_ipt w_180'))?>
    <?php echo CHtml::submitButton('搜索',array('class'=>'mem_bgx mem_btn01'))?>
    <?php $this->endWidget(); ?>
    </div>

  **Controller里面要有如下代码：**
    
    
    $product = New Product();
    $product->unsetAttributes();//清空属性
    $product->id = $id;
    if(isset($_GET['Product'])){
    	$product->attributes = $_GET['Product'];
    }

**model** 基本是用的是Search 不需要改什么。注意：compare后面有参数true则为模糊搜索，实例： 
    
    
    $criteria->compare('name',$this->name,true);