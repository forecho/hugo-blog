---
title: "Mac 升级 Yosemite 之后 PHP 环境修复"
date: 2014-10-19T22:06:00+08:00
tags: ["PHP"] 
draft: false
toc: true
---
Yosemite 终于发布了，我第一时间升级了。但是没想到 PHP 环境受到影响了，直到现在终于修复好了。

##修复 Apache

Apache 好像是换新版本了，升级完系统之后项目打不开了。之前设置好的虚拟主机都无效了。

首先打开 `sudo vim /etc/apache2/httpd.conf`  这个文件，

### 开启虚拟主机功能，去掉下面代码的「#」注释：

```
#Include /private/etc/apache2/extra/httpd-vhosts.conf
```
改过的代码如下：

```
Include /private/etc/apache2/extra/httpd-vhosts.conf
```

### 如果想开启伪静态，去掉下面代码的「#」注释：

```
#LoadModule rewrite_module libexec/apache2/mod_rewrite.so
```
改过的代码如下：

```
LoadModule rewrite_module libexec/apache2/mod_rewrite.so
```
### 如果想使用系统自带的 PHP 版本的话，去掉下面代码的「#」注释：

```
#LoadModule php5_module libexec/apache2/libphp5.so
```
改过的代码如下：

```
LoadModule php5_module libexec/apache2/libphp5.so
```

以上操作完了之后记得要重启Apache:

```sh
sudo apachectl restart
```

##修复虚拟主机
<!--more-->
现在我们虽然能用虚拟主机了，但是好像不能解析 PHP 了，打开 `/private/etc/apache2/extra/httpd-vhosts.conf` 文件在每一个配置里面添加一行下面的代码就可以了：

`Require all granted`

示例如下：

```
<Directory "/Users/$USER/Sites/">
Options Indexes MultiViews FollowSymLinks
AllowOverride All
Require all granted
</Directory>
```
记得要重启 Apache：

```sh
sudo apachectl restart
```
##安装 PHP Mcrypt 扩展

这个花费我很长时间，主要是我本来还想用 brew 安装的，省事，但是还是没效果，最后没办法还是编译安装了。

```sh
cd ~/Downloads
wget https://github.com/php/php-src/archive/PHP-5.5.14.zip
unzip PHP-5.5.14.zip
cd php-src-PHP-5.5.14/ext/mcrypt/
/usr/bin/phpize
./configure
make
sudo make install
```
然后拷贝一份 php.ini 配置文件，重装系统这个文件没了：

```sh
sudo cp /etc/php.ini.default /etc/php.ini
```
然后记得在 php.ini 文件里面添加这个扩展，如下面代码：

```
extension = mcrypt.so
```



以上操作完之后你的 PHP 环境应该又回来了。：）

—————–update 2014年11月02日—————-

Yosemite 自带编译安装之后的 PHP 环境 GD 库不支持 png 和 FreeType ，会导致各种验证码出不来，坑。

一键解决办法如下（重新编译使用 PHP5.6）：

```sh
curl -s http://php-osx.liip.ch/install.sh | bash -s 5.6
```

##参考文章：

1. [Apache, MySQL & PHP on OS X Yosemite](http://tobschall.de/2014/08/04/yosemite-mamp/)
2. [How to Manually Build & Install php-mcrypt on Mac OS X](http://digitizor.com/2014/06/29/build-install-php-mcrypt-mac-os-x-manually/)
3. [Installing and Configuring Apache, PHP and MySQL on OSX 10.10 Yosemite](http://www.pixelfolio.co.uk/blog/installing_and_configuring_apache_php_mysql_on_yosemite)
4. [After upgrade, PHP no longer supports PNG operations](http://stackoverflow.com/questions/26443242/after-upgrade-php-no-longer-supports-png-operations)