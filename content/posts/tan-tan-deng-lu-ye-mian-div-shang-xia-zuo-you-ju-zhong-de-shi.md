---
title: "谈谈登陆页面Div上下左右居中的事"
date: 2012-03-02T15:25:00+08:00
tags: ["css"] 
draft: false
toc: true
---

最主要的CSS样式

```css
.login{
    width:631px;
    height:314px;
    margin-top:-157px;
    margin-left:-316px;
    position: absolute;
    top:50%;
    left:50%;
    background:url(images/mod.gif);
}
```

使用 **绝对定位 ** 然后margin-top选取div height的一半值 margin-left 选取div width的一半值  

其他方法参考[这里~](http://demo.tutorialzine.com/2010/03/centering-div-vertically-and-horizontally/demo.html)