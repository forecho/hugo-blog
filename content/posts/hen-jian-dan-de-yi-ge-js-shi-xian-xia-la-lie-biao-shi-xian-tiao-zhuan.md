---
title: "很简单的一个js实现下拉列表实现跳转"
date: 2012-03-03T19:43:00+08:00
tags: ["javascript"] 
draft: false
toc: true
---

直接上效果页面，[请猛击~](http://www.nowhisky.com/demo/select.html) 代码很简单，完整版的HTML代码，如下：

```html
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
    <title>下拉菜单实现页面跳转</title>
    </head>
    <body>
    <select name="menu122" onchange="if(this.options[this.selectedIndex].value!=''){window.open(this.options[this.selectedIndex].value,'_self');}">
    <option value="#">友情链接站点</option>
    <option value="http://www.baidu.com">百度</option>
    </select>
    </body>
    </html>
```

或者用jQuery这样写：

```html
    <select id="dynamic_select">
        <option value="" selected>Pick a Website</option>
        <option value="http://www.google.com/">Google</option>
        <option value="http://www.youtube.com/">YouTube</option>
        <option value="http://www.stackoverflow.com/">Stack Overflow</option>
    </select>

    <script>
        $(function(){
          // bind change event to select
          $('#dynamic_select').bind('change', function () {
              var url = $(this).val(); // get selected value
              if (url) { // require a URL
                  window.location = url; // redirect
              }
              return false;
          });
        });
    </script>
```

## Comments

**[yokelai](#83 "2012-03-05 19:49:14"):** 当选项不为空时执行事件,学习啦！

