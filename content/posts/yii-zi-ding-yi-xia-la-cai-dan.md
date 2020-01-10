---
title: "yii自定义下拉菜单"
date: 2012-11-06T15:51:00+08:00
categories: 
draft: false
toc: true
---

有时候我们需要自定义下来菜单，如果数据是从数据库的另外一张表读取的话，你可以参考我的[这篇文章](/archives/522)。 但是现在我们只是要简单的做个下拉菜单，数据并不需要从数据库读取，自己定义即可，那么怎么样修改呢？其实也很简单，找到相应view文件夹下面的_form.php文件，找到下面这行代码： 
    
    
    <?php echo $form->textField($model,'type'); ?>

修改成： 
    
    
    <?php echo $form->dropDownList( $model,'type', array('0' => '菜单分类', '1' => '单页面'));?>

这时候页面生成的是如下HTML: 
    
    
    <select name="Category[type]" id="Category_type">
        <option value="0">菜单分类</option>
        <option value="1">单页面</option>
    </select>