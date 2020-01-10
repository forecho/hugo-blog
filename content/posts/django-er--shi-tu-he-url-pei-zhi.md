---
title: "Django(二) - 视图和URL配置"
date: 2013-05-04T16:29:00+08:00
categories: 
draft: false
toc: true
---

Django的URl都是需要单独设计的，每个URL指定一个视图中的函数。 Django和URL配置背后的哲学：松耦合原则（决定URL返回哪个视图函数和实现这个视图函数是在两个不同的地方。 这使得开发人员可以修改一块而不会影响另一块。） **永远的Hello world** 首先找到`urls.py`文件，代码修改为： 
    
    
    from django.conf.urls.defaults import *  #导入django.conf.urls.defaults下的所有模块
    from views import hello  #从mysite/views.py模块中引入了hello视图。
    
    #第二行调用 patterns() 函数并将返回结果保存到 urlpatterns 变量
    urlpatterns = patterns('',
        (r'^hello/$', hello), #第一个元素是模式匹配字符串（正则表达式）；第二个元素是那个模式将使用的视图函数。
    )

_注：正则表达式字符串的开头字母“r”。 它告诉Python这是个原始字符串，不需要处理里面的反斜杠（转义字符）。例如：“\n”是两个字符串：“\”和“n”。_ 然后新建一个`views.py`文件（跟urls.py同级目录），写入如下代码： 
    
    
    from django.http import HttpResponse  #从 django.http 模块导入（import） HttpResponse 类
    
    #hello 的视图函数 每个视图函数至少要有一个参数，通常被叫作request。
    def hello(request):
        return HttpResponse("Hello world")

运行命令行 `python manage.py runserver` 打开你的浏览器访问 <http://127.0.0.1:8000/hello/> 如果你看到了 Hello world 页面，那就表示你成功了。 **附件：正则表达式** ![正则表达式](http://m2.img.libdd.com/farm5/2013/0115/21/86FA1A22D0DEAF0365359189033DD492AAE58F7491351_592_302.PNG)