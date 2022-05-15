---
title: "浅谈 CSS 的一些写法"
date: 2013-05-15T15:57:00+08:00
categories: 
draft: false
toc: true
---

开发网站的时候，DIV 最好是先用上 class，因为 id 的权限比 class 要高。 写 class 样式的的时候最好也要带上父级的 class 名称。比如： 
    
    
```css
.header .topbar{ padding-top:10px;}
.nav_bar .main_nav li{ float:left;}
```

写 div 的时候尽量少用 float 的，因为很多莫名其妙的兼容性问题都可能是浮动害的。 `clear: both;` 是个好东西。 最重要的是写语义化的 CSS，让人一看就能看出来是什么意思。比如： 
    
    
```css
.fl { float: left; }
.mt10 { margin-top: 10px; }
.w980 { width: 980px; }
```

这个语义化 CSS 很实用，就举这三个例子。 以后知道怎么用 CSS 了吧？