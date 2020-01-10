---
title: "jQuery Ajax this的问题"
date: 2013-06-05T10:31:00+08:00
categories: 
draft: false
toc: true
---

写了一个鼠标点击按钮Ajax效果，代码如下： 
    
    
    <script type="text/javascript">
    //报名参加活动
    $('.right a').bind('click',function(){
        var id = $(this).next('.d_n').text();
        $.ajax({
            url:"<?php echo Yii::app()->createUrl('/ajax/AjaxJoinActivity/') ?>",
            type:'POST',
            data:'id='+id,
            success:function(msg){
                $(this).html(msg);
            }
        });
    })
    </script>

但是这样写会有个问题，就是`$(this)`没有效果，Google找了一下解决方案，如下： 1、添加一行代码：`context: this，`修改之后的代码如下： 
    
    
    <script type="text/javascript">
    //报名参加活动
    $('.right a').bind('click',function(){
        var id = $(this).next('.d_n').text();
        $.ajax({
            context: this,
            url:"<?php echo Yii::app()->createUrl('/ajax/AjaxJoinActivity/') ?>",
            type:'POST',
            data:'id='+id,
            success:function(msg){
                $(this).html(msg);
            }
        });
    })
    </script>

2、转换一下`this`变量，重新定义。改过之后的代码如下： 
    
    
    <script type="text/javascript">
    //报名参加活动
    $('.right a').bind('click',function(){
        var id = $(this).next('.d_n').text();
        var that = this;
        $.ajax({
            url:"<?php echo Yii::app()->createUrl('/ajax/AjaxJoinActivity/') ?>",
            type:'POST',
            data:'id='+id,
            success:function(msg){
                $(that).html(msg);
            }
        });
    })
    </script>

以上都能解决问题，看你喜欢那种了。 参考资料如下： <http://stackoverflow.com/questions/6394812/this-inside-of-ajax-success-not-working> <http://stackoverflow.com/questions/1570146/ajax-jquery-success-scope>