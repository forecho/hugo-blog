---
title: "yii 调用KindEditor在线编辑器"
date: 2012-09-28T15:33:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

1. 先下载[这个](http://pan.baidu.com/share/link?shareid=8839&uk=2684558169)文件，然后解压，把editor 这个文件夹放在你项目的根目录里。
2. 然后找_form.php这个文件，找到你的"content"（这里我的是名称是“news_content”），修改成如下代码：（被注释掉的是原来的代码，上面一行是新加上的。）

```php
<div class="row">
    <?php echo $form->labelEx($model,'news_content'); ?>
    <?php echo $form->textArea($model,'news_content',array('style'=>'width:90%; height:500px')); ?>
    <?php // echo $form->textField($model,'news_content',array('size'=>60,'maxlength'=>8000)); ?>
    <?php echo $form->error($model,'news_content'); ?>
</div>
```

然后在_form.php这个文件开头，添加如下代码，调用编辑器样式。

```php
<?php
    Yii::app()->clientScript->registerCssFile('./editor/themes/default/default.css');
?>
```

最后在_form.php这个文件结尾处，添加如下代码，调用编辑器的js文件以及调用js

```javascript
<script charset="utf-8" src="./editor/kindeditor.js"></script>
<script type="text/javascript">
    KindEditor.ready(function(K) {
        editor = K.create('#News_news_content', {
            allowFileManager: true,
            resizeType: 1,
            newlineTag: 'p',
            //syncType: '',
            // uploadJson: '<?php echo $this->createUrl('news/upload') ?>'
        });
    });
</script>
```