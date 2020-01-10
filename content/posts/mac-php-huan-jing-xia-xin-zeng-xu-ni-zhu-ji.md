---
title: "Mac PHP 环境下新增虚拟主机"
date: 2015-01-16T22:30:00+08:00
tags: ["php"] 
draft: false
toc: true
---
注：这里只是新增，如果是第一次开启虚拟主机，请参考[这篇文章](http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/)。

1、运行`sudo vim /etc/apache2/extra/httpd-vhosts.conf`，就打开了配置虚拟主机文件httpd-vhost.conf，配置虚拟主机了。增加虚拟主机代码如下：

```
<VirtualHost *:80>
    DocumentRoot "/Users/[用户名]/Sites/site"
    ServerName www.site.com
    ErrorLog "/private/var/log/apache2/sites-error_log"
    CustomLog "/private/var/log/apache2/sites-access_log" common
    <Directory />
                Options Indexes FollowSymLinks MultiViews
                AllowOverride All
                Order deny,allow
                Allow from all
    </Directory>
</VirtualHost>
```

2、保存退出，并重启Apache。

```sh
sudo apachectl restart
```

3、运行`sudo vim /etc/hosts`，打开hosts配置文件，加入「127.0.0.1 www.site.com」，
这样就可以配置完成sites虚拟主机了，访问「http://www.site.com」就相当于访问`/Users/[用户名]/Sites/site`文件下面的项目了。


补充：Mac默认PHP配置文件在 `/private/etc/php.ini`