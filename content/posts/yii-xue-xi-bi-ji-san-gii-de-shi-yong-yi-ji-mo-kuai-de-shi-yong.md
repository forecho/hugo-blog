---
title: "yii学习笔记（三）-gii的使用以及模块的使用"
date: 2012-07-10T11:28:00+08:00
tags: ["yii"] 
draft: false
toc: true
---

**gii的使用** gii可以帮你生成很多文件，很方便。

访问：[http://localhost/blog/index.php?r=gii](http://localhost/blog/index.php?r=newstype)  输入之前设置的密码，进入gii。

选择 `Model Generator`在Table Name下面输入数据库 表名称，然后点Preview 预览，点Generate 生成文件。

然后在左边再选择`Crud Generator`在Model Class 下面输入你刚才生成对应的controllers文件（也就是controllers文件夹对应的文件名，注意大小写要匹配），然后点Preview 预览，点Generate 生成文件。

**模块的使用（就是使前后台分离）**

在gii左边选择Module Generator 在Module ID下面输入Admin，然后点Preview 预览，点Generate 生成文件。这个时候你会发现protected文件夹下面自动生成了一个modules文件夹。

然后修改main.php配置文件。在gii这个数组下面添加：

```
'Admin'=>array(
    'class'=>'application.modules.Admin.AdminModule',
),
```

那么接下来就可以在gii中给Admin添加模块了。

在左边再选择`Crud Generator`在Model Class 下面输入对应的controllers文件（也就是controllers文件夹对应的文件名，注意大小写要匹配），但是注意要修改Controller ID的内容，要在前面加上 “`Admin/`” ，然后点Preview 预览，点Generate 生成文件。