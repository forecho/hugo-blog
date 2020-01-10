---
title: "01、jQuery双色表格"
date: 2011-12-08T14:48:00+08:00
tags: ["javascript"] 
draft: false
toc: true
---

js代码：

```javascript
$(document).ready(function(){
    //如果鼠标移动到class为stripe的表格的tr上时，执行函数
    $(".stripe tr").mouseover(function(){
        //给这行添加class值为over并且当鼠标经过该行时执行函数
            $(this).addClass("over");
        }).mouseout(function(){
            $(this).removeClass("over");//移除该行的class
        })
    $(".stripe tr:even").addClass("alt");//给class为stripe的表格的偶数添加class值为alt
});
```
知识点：

  * mouseover(fn) 在每一个匹配元素的mouseover事件中绑定一个处理函数。mouseover事件会在鼠标移入对象时触发。
  * mouseout(fn) 在每一个匹配元素的mouseout事件中绑定一个处理函数。mouseout事件在鼠标从元素上离开后会触发。
  * removeClass(_[class]_) 从所有匹配的元素中删除全部或者指定的类。
  * addClass(class) 为每个匹配的元素添加指定的类名。一个或多个要添加到元素中的CSS类名，请用空格分开。