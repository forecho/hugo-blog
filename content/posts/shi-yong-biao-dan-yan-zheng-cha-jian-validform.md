---
title: "使用表单验证插件 Validform"
date: 2012-10-16T13:52:00+08:00
tags: ["jQuery"] 
draft: false
toc: true
---

[官方网址](http://validform.rjboy.cn/) 先下载完整的示例文档。 调用如下两个文件（注意文件路径）：

```javascript
<script type="text/javascript" src="resources/scripts/jquery-1.6.2.min.js"></script>
<script type="text/javascript" src="resources/scripts/Validform_v5.1_min.js"></script>
```

然后添加javascript代码（此处我使用的是示例2）：

```javascript
<script type="text/javascript">
$(function(){
    //$(".registerform").Validform();  //就这一行代码！;

    $(".registerform").Validform({
        tiptype:2
    });
})
</script>
```

然后是表单的视图页面主要代码：

```html
<p>
    <label>导航名称</label>
    <input class="text-input small-input" type="text" id="small-input" name="name" datatype="s2-10" errormsg="昵称至少2个字符,最多10个字符！" />
    <span class="Validform_checktip"></span>
    <br />
</p>
```

**form表单的class是"registerform"**。即：

```html
class="registerform"
```

PS: 此处我表单的input与提示文字Validform_checktip 属于同一级标签，而官方给我们的是都有一个父级的td标签，所以我需要改官方的Validform_v5.1_min.js文件，找到下图代码 ![](http://m1.img.libdd.com/farm4/2012/0914/10/6490CA006AB38112DE4FE6C76AD2177A5CAFE5189977_800_86.jpg) 修改如下：

```javascript
if(type==2 && o.obj){
o.obj.next(".Validform_checktip").html(msg);
    Validform.util.cssctl(o.obj.next(".Validform_checktip"),o.type);
}
```

**即：删掉两处的parent和find。** 更多的说明文档，请参考[这里](http://validform.rjboy.cn/?cat=1)。   样式的话，只需调用如下三个即可。

```css
/* 表单 */
.Validform_wrong {
    color: red;
    padding-left: 20px;
    white-space: nowrap;
    background: url(../images/error.png) no-repeat left center;
}
.Validform_right {
    color: #71B83D;
    padding-left: 20px;
    background: url(../images/right.png) no-repeat left center;
}
.Validform_checktip {
    margin-left: 8px;
    line-height: 20px;
    height: 20px;
    overflow: hidden;
    color: #999;
    font-size: 12px;
}
```