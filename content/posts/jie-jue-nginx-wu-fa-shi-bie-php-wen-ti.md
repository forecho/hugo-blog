---
title: "解决Nginx无法识别PHP问题"
date: 2013-07-23T17:27:00+08:00
tags: ["Nginx", "PHP基础"] 
draft: false
toc: true
---

这两天很蛋疼的在Linux下面配置LNMP环境。参考了比较多的资料，但是发现一个问题，就是安装好环境之后，Nginx无法识别PHP文件，HTML文件是没有问题的。

还要解决配置多个虚拟目录的问题。

后来就各种Google找资料，按照[这篇文章](http://www.markdream.com/server/nginx-config-null-host.shtml)的方法配置，还算比较成功。

这个可以成功的配置虚拟目录，但是还是无法识别PHP。

后来找到[这篇文章](http://ilovers.sinaapp.com/drupal/node/10)，其中提到：Nginx 本身并没有PHP 解释器，所以，需要借助于 PHP 提供的 fastcgi，所以，需要运行 PHP 的 php-cgi 进程；

```
php-cgi -b 127.0.0.1:9000 # cmd 中运行 php-cgi，监听本地地址，9000端口；
```
我是比较头晕不知道自己是否安装了这个，但是发现自己Nginx配置目录下还是有fastcgi文件的。 **我的9000端口法无监听？**试了一下这个方法，无效。 其实这个地方监听是什么，你需要去看`/etc/php5/fpm/pool.d/www.conf`配置文件写的是什么，这个写成一致即可。 后来把： 

```
location ~ \.php($|/) {
    set  $script     $uri;
    set  $path_info  "";
    if ($uri ~ "^(.+\.php)(/.+)") {
      set  $script     $1;
      set  $path_info  $2;
    }
    fastcgi_pass   127.0.0.1:9000;
    include        fastcgi_params;
    fastcgi_param  PATH_INFO                $path_info;
    fastcgi_param  SCRIPT_FILENAME          /usr/local/vhost/demo$script;
    fastcgi_param  SCRIPT_NAME              $script;
}
```
改成为：

```
location ~ \.php$ {
    root   /usr/local/vhost/demo;
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```
然后重启Nginx服务，访问你设置的server_name ，成功


补充一些参考文章：

- [Linux上配置Nginx+PHP5(FastCGI)](http://www.laruence.com/2009/07/28/1030.html)
- [ubuntu 12.10 默认安装php5-fpm无监听9000端口，nginx无法链接php5-fpm修正](http://blog.sina.com.cn/s/blog_6a0b2afd01014acf.html)
- [Installing Nginx and PHP on Linux Mint 13](http://ordinary-linux-user.blogspot.com/2013/04/installing-nginx-and-php-on-linux-mint.html)
- [How to Install Linux, nginx, MySQL, PHP (LEMP) stack on Ubuntu 12.04](https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04)