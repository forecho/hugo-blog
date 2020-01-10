---
title: "使用Yii的 CMenu"
date: 2014-11-17T12:51:00+08:00
tags: ["yii1"] 
draft: false
toc: true
---
```php
$this->widget('zii.widgets.CMenu',array(
    'activeCssClass'=>'当前热点元素的样式',
    'firstItemCssClass'=>'第一个元素的样式',
    'lastItemCssClass'=>'最后一个元素的样式',
    'htmlOptions'=>array('class'=>'默认样式'),
    'items'=>array(
        array(
            'label'=>'网站概况',
            'url'=>array('/admin'),
            'itemOptions'=>array(
                'class'=>'li_status'
            ),
            'active'=>$this->id=='admin'?true:false
        ),
        array(
            'label'=>'图片管理',
            'url'=>array('/picture'),
            'template'=>'{menu}<span>this is additional infomation</span>',
            'itemOptions'=>array('class'=>'li_picture'),
            'active'=>$this->id=='picture'?true:false,
            'visible'=>true
        ),
        array('label'=>'管理员管理',
            'url'=>array('/manager'),
            'itemOptions'=>array('class'=>'li_manager'),
            'submenuOptions'=>array('class'=>'subclass'),
            'active'=>($this->id=='manager' && $this->action->id!='changepswd')?true:false,
            'visible'=>false
        ),
        array(
            'label'=>'密码修改',
            'url'=>array('/manager/changepswd'),
            'linkOptions'=>array('target'=>'_blank'),
            'itemOptions'=>array(
                'class'=>'li_changepswd'
            ),
            'items'=>array(
                array('label'=>'子栏目')
            ),
            'active'=>($this->id=='manager' && $this->action->id=='changepswd')?true:false,
            'visible'=>true
        ),
        array(
            'label'=>'登陆',
            'url'=>array('/site/login'),
            'itemOptions'=>array('class'=>'li_login'),
            'visible'=>Yii::app()->user->isGuest
        ),
        array(
            'label'=>'退出 ('.Yii::app()->user->name.')',
            'url'=>array('/site/logout'),
            'itemOptions'=>array('class'=>'li_login'),
            'visible'=>!Yii::app()->user->isGuest
        )
    ),
));
```
**说明：**
<!--more-->
1. `label`：菜单显示的文本，可以加 `html` 进行修饰，但要将 `encodeLabel` 参数值设为 `false`

2. `url`：链接地址，若是字符串，则是基于网站根地址的绝对路径，比如网站地址为`baidu.com`，字符串url设置为`article`，则最终生成的地址为`baidu.com/article`，如果设置类型为数组，则效果与`createUrl`方法一样，比如网址还是`baidu.com`，设置的数组url为`array(detail/article)`，则最终生成的地址为`baidu.com/?r=detail/article`，控制器/方法格式的

3. `visible`：可见，`boolean`值，当然可以用函数来取值，决定什么情况下隐藏

4. `active`：正在访问，`boolean`值，如果是`true`，会在相应`li`中加入`active`样式，上面代码用到`$this->id`是个很好用的方法

5. `items`：定义子目录，`array()`，通过样式可定义收缩排列或者鼠标经过时显示子目录

6. `template`：模板，模板中用`{menu}`来代表替换内容，见上代码

7. `linkOptions`：`<a>`的属性，可定义`class`，`rel`，`target`等属性，见上代码

8. `itemOptions`：`<li>`的属性，可定义 `class`等属性，见上代码

9. `submenuOptions`：子栏目的`<ul>`属性，`<li>`和`<a>`属性还是和上面一样分别对`item`设置

10. `activeCssClass`：当前选中菜单的Css的`Class`名称

11. `firstItemCssClass`：第一个菜单按钮的Css的`Class`名称

12. `lastItemCssClass`：最后一个菜单按钮的Css的`Class`名称

当然可以分别为每个Item菜单元素添加指定的`Class`，即在对应的Item元素上增加`itemOptions`设置。

