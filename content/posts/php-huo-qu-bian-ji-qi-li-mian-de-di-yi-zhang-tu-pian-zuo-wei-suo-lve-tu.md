---
title: "PHP获取编辑器里面的第一张图片做为缩略图"
date: 2011-11-23T17:11:00+08:00
tags: ["php"] 
draft: false
toc: true
---

思路是这样的：使用正则表达式 查询到文章所在的字段，获取到图片的绝对路径。 代码如下：（这个我使用的是CI框架开发的，下面是控制器里面的代码）


```php
//获取文章字段的内容

$data['sel_news'] = $this->mhome->sel_news();

foreach ($data['sel_news'] as $row)
{
    $row['content'];
}
//正则表达式查找图片的绝对路径，并且获取
preg_match_all ("/<(img|IMG)(.*)(src|SRC)=[\"|'|]{0,}([h|\/].*(jpg|JPG|gif|GIF|png|PNG))[\"|'|\s]{0,}/isU",$row['content'],$out);
$data['get_image'] = $out[4];
```


视图输出代码：

```html
<img src='<?php echo $get_image[0];?>'></img>
```

如是是想输出多张图片的话，视图就用foreach 输出。