---
title: "Mac OS 上安装MySQLdb一系列问题"
date: 2013-04-24T23:12:00+08:00
categories: 
draft: false
toc: true
---

话说装这个插件非常麻烦。 首先你得先装MySQL。先去下载64 位的MySQL，然后安装。步骤你可以参照[这篇文章~](http://yan-yan.info/2011/install-mysql-5.5-on-mac-os-10.7-with-python-support.html) 其实他写的已经很详细了，但是我安装的过程中遇到的很多问题。 声明一下：我是下载MySQLdb然后手动安装的，没有用 easy_install去安装。 **1、**报错： 
    
    
    sh: mysql_config: command not found
    ......
    ......
    EnvironmentError: mysql_config not found

解决办法是：在MySQLdb源码里面找到site.cfg 修改为： 
    
    
    mysql_config = /usr/local/mysql/bin/mysql_config

然后python setup.py install **2、**报错： 
    
    
    ......
    import _mysql
    ......
    Reason: image not found

解决方法是：需要解决动态引入的问题了（重新做一遍，并且设置环境变量）： 
    
    
    $ python setup.py clean
    $ python setup.py build
    $ python setup.py install
    $ export DYLD_LIBRARY_PATH=/usr/local/mysql/lib:$DYLD_LIBRARY_PATH

或者运行下面两行命令： 
    
    
    sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib/libmysqlclient.18.dylib
    
    sudo ln -s /usr/local/mysql/lib /usr/local/mysql/lib/mysql

参考资料：[http://stackoverflow.com/questions/4730787/python-import-mysqldb-error-mac-10-6 ](http://stackoverflow.com/questions/4730787/python-import-mysqldb-error-mac-10-6 ) **3、**报错： error: command 'clang' failed with exit status 1报错的时候 解决方法是：去Xcode的Preference的Download下的Components下载command line tool **4、**在$ python manage.py syncdb 的时候报错： 
    
    
    ImportError: cannot import name smart_unicode

解决办法是：在「/Library/Python/2.7/site-packages/tinymce/widgets.py」文件里找到下面代码： 
    
    
    from django.forms.util import smart_unicode

替换为： 
    
    
    from django.utils.encoding import smart_unicode