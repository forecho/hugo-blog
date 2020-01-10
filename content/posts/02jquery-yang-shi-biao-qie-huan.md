---
title: "02、jQuery样式表切换"
date: 2011-12-09T14:02:00+08:00
tags: ["javascript"] 
draft: false
toc: true
---

JS代码如下：

```javascript
(function($)
{
    $(document).ready(function() {
        $('.styleswitch').click(function()
        {
            switchStylestyle(this.getAttribute("rel"));
            return false;
        });
        var c = readCookie('style');
        if (c) switchStylestyle(c);
    });

    function switchStylestyle(styleName)
    {
        $('link[rel*=style][title]').each(function(i)
        {
            this.disabled = true;
            if (this.getAttribute('title') == styleName) this.disabled = false;
        });
        createCookie('style', styleName, 365);
    }
})(jQuery);

function createCookie(name,value,days)
{
    if (days)
    {
        var date = new Date();
        date.setTime(date.getTime()+(days*24*60*60*1000));
        var expires = "; expires="+date.toGMTString();
    }
    else var expires = "";
    document.cookie = name+"="+value+expires+"; path=/";
}
function readCookie(name)
{
    var nameEQ = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++)
    {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(nameEQ) == 0) return c.substring(nameEQ.length,c.length);
    }
    return null;
}
function eraseCookie(name)
{
    createCookie(name,"",-1);
}
```

效果虽然有了，但有点看不懂，求解释~~~

## Comments

**[29的博客](#25 "2011-12-09 22:19:13"):** 学习了的。

