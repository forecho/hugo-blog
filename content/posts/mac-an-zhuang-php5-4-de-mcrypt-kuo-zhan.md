---
title: "Mac 安装 PHP5.4 的 Mcrypt 扩展"
date: 2014-03-01T14:34:00+08:00
tags: ["php", "mac"] 
draft: false
toc: true
---

忘记什么时候把 MacBook 的 PHP 版本升级到 PHP5.4了，然后准备学一下 Laravel 这个框架的时候，提醒 Mcrypt 扩展未安装。

安装这个扩展真是头疼，尽管有 Homebrew ，尽管能 Google 到很多英文很好的教程，有简单的命令的，有复杂命令的，但是照着教程敲代码还是安装不成功。

在终端输入 php -v 的时候，会出现下面报错代码：


```sh
PHP Warning:  PHP Startup: Unable to load dynamic library '/usr/lib/php/extensions/no-debug-non-zts-20100525/mcrypt.so' - dlopen(/usr/lib/php/extensions/no-debug-non-zts-20100525/mcrypt.so, 9): image not found in Unknown on line 0
PHP 5.4.17 (cli) (built: Aug 25 2013 02:03:38)
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies
```

但是我 /etc/php.ini 配置文件里面根本没有 /usr/lib/php/extensions/no-debug-non-zts-20100525/ 这个路径，也不记得这个东西在哪？怎么修改？

剩下的我只能继续 Google，找几份教程一起来，然后我发现了这个：[Mcrypt installer for OS X 10.8/10.9](http://topicdesk.com/downloads/mcrypt/mcrypt-download) 不注意的话，估计会忽略最下面的[下载链接](http://downloads.topicdesk.com/installers/topicdesk_Mcrypt_Installer_1.0.zip)。

我下载下来试了一下，安装步骤就跟安装软件是一样的，一直点下一步就好了，然后我在输出 phpinfo() 的页面，竟然搜索到了 Mcrypt ，这就说明我安装成功了，然后试了一下 Laravel 首页，果然成功了。

还是这个方便省事，感谢这个软件。

最后分享一个学 Laravel 的资源：[https://github.com/maliang/LikeLaravel ](https://github.com/maliang/LikeLaravel )