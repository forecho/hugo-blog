---
title: "Mac OS 上安装MySQLdb一系列问题"
date: 2013-04-24T23:12:00+08:00
categories: 
draft: false
toc: true
---

话说装这个插件非常麻烦。 首先你得先装 MySQL。先去下载 64 位的 MySQL，然后安装。步骤你可以参照[这篇文章~](http://yan-yan.info/2011/install-mysql-5.5-on-mac-os-10.7-with-python-support.html) 其实他写的已经很详细了，但是我安装的过程中遇到的很多问题。 声明一下：我是下载 MySQLdb 然后手动安装的，没有用 easy_install 去安装。

### 报错：

```sh
sh: mysql_config: command not found
......
......
EnvironmentError: mysql_config not found
```

解决办法是：在 MySQLdb 源码里面找到 site.cfg 修改为：

```sh
mysql_config = /usr/local/mysql/bin/mysql_config
```

然后 python setup.py install

### 报错：

```sh
......
import _mysql
......
Reason: image not found
```

解决方法是：需要解决动态引入的问题了（重新做一遍，并且设置环境变量）：

```sh
$ python setup.py clean
$ python setup.py build
$ python setup.py install
$ export DYLD_LIBRARY_PATH=/usr/local/mysql/lib:$DYLD_LIBRARY_PATH
```

或者运行下面两行命令：

```sh
sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib/libmysqlclient.18.dylib
sudo ln -s /usr/local/mysql/lib /usr/local/mysql/lib/mysql
```

参考资料：[http://stackoverflow.com/questions/4730787/python-import-mysqldb-error-mac-10-6 ](http://stackoverflow.com/questions/4730787/python-import-mysqldb-error-mac-10-6)

### 报错：

`error: command 'clang' failed with exit status 1` 报错的时候 解决方法是：去 Xcode 的 Preference 的 Download 下的 Components 下载 command line tool

### `$ python manage.py syncdb` 的时候报错：

```sh
ImportError: cannot import name smart_unicode
```

解决办法是：在「/Library/Python/2.7/site-packages/tinymce/widgets.py」文件里找到下面代码：

```sh
from django.forms.util import smart_unicode
```

替换为：

```sh
from django.utils.encoding import smart_unicode
```