---
title: "yii修改默认分页每页显示条数"
date: 2012-11-06T13:55:00+08:00
categories: 
draft: false
toc: true
---

yii自带默认分页是每页显示10条信息。 如果要修改每页显示条数，我们先找到这个页面的**相应的Models文件**，找到**search**这个方法，然后找到下面这块代码： 
    
    
    return new CActiveDataProvider($this, array(
        'criteria'=>$criteria,
    ));

改成： 
    
    
    return new CActiveDataProvider($this, array(
        'criteria'=>$criteria,
        'pagination'=>array(
            'pageSize'=>2, //代表每页显示2条信息
        ),
    ));