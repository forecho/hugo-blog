---
title: "用jQuery实现当前页面给菜单导航一个特定样式"
date: 2013-07-22T17:50:00+08:00
tags: ["jQuery"] 
draft: false
toc: true
---

以前一直困惑我的一个问题就是：怎么实现给当前页面导航菜单一个样式？

最最开始的时候用的是最笨的方法，就是每个导航页面都写得不一样。比方说index.html页面的时候，会在index导航的a标签一个.active样式，然后第二个Posts.html页面的时候会在Posts导航的a标签一个.active样式。这个虽然效果实现了，但是有个很大的缺点 - 导航不能调用一个，每个页面的导航写法不一样。 

后来发现可以用PHP的GET变量来实现这个效果。这个效果比上一个好，可以调用同一个导航文件，但是现在想想，这个方法会让导航这个文件代码变得臃肿，每个a标签都要写个if语句判断语句。然后根据GET获得的变量来判断是否显示.active这个样式。 

今天无意中看了下一个[开源的后台模板](http://usman.it/themes/charisma/)。下载下来发现他这个是纯HTML也实现了这个功能，然后查看源代码，在[charisma.js](http://usman.it/themes/charisma/js/charisma.js)文件发现了下面这个关键的代码： 

```javascript
//highlight current / active link
$('ul.main-menu li a').each(function(){
	if($($(this))[0].href==String(window.location))
		$(this).parent().addClass('active');
});
```

然后根据这个，自己写了个Demo，果然实现了。这下省了不少代码，也解决了一个我一直以来比较困惑的问题，看来还是得多看看别人的代码，学习。

[Demo下载](http://pan.baidu.com/share/link?shareid=1247541698&uk=2684558169)。

* * *

其实这样也有效果：
```javascript
$('ul.nav li a').each(function(){
    if ($(this)[0].href == window.location.href) {
        $(this).parent().addClass('active');
    }
});
```
## Comments

**[风逐蓝天](#154 "2013-07-22 21:58:00"):** Good. 学习了.

**[风逐蓝天](#155 "2013-07-25 16:08:00"):** $($(this))[0].href 这个是什么意思？ 为什么不能是 $(this)[0].href ？

**[ForEcho](#156 "2013-07-25 16:22:00"):** 这个问题我也想过，但是没搞清楚，毕竟不是专业的前端。但是我试过$(this)[0].href，结果就是行不通。你可以试着alert($(this)[0].href) 查看结果。

**[gao](#191 "2013-11-12 17:55:00"):** 优化后:
```javascript
$(".nav li a").each(function(){
    $this = $(this);
    if($this[0].href==String(window.location)){
        $this.parent().addClass("current");
    }
});
```
**[ForEcho](#192 "2013-11-12 18:29:00"):** Thx 这样写更美观了

