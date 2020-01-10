---
title: "把项目移植到Ubuntu上遇到的一些坑 - PHP5.4降级到PHP5.3"
date: 2013-06-25T09:39:00+08:00
categories: 
draft: false
toc: true
---

其实在Ubuntu上搭建PHP环境不是很难，可以参考[这篇文章-手把手教你在Ubuntu上安装Apache、MySql和PHP](http://developer.51cto.com/art/201110/299303.htm)。 PHP 默认是没有开启报错信息的，需要手动开启，可以参考[这篇文章-开启Ubuntu下的PHP错误提示](http://deloz.net/1000000566.html)。 环境搭建好之后，运行项目发现有很多错误，很多坑，有点莫名其妙的感觉。 Google问题之后也不知道如何解决，后来找到了[这篇文章- PHP5.4中一个需要注意的变化(Chained string offsets) ](http://www.laruence.com/2011/11/28/2317.html)。 这才发现可能是我的PHP 版本问题，Ubuntu默认安装的是PHP5.4版本，这个版本较之前的PHP5.3有一些改动。 于是找到了[这篇文章-Ubuntu 12.10 php5.4 降级到 php5.3脚本](http://www.ubuntugeek.com/how-to-downgrade-php-version-from-5-4-to-5-3-in-ubuntu-12-10-quantal.html)。 大致就是先下载一个[文件](http://www.ubuntugeek.com/images/php5_4_downgrade_5.3.sh)，然后修改这个文件的权限： 
    
    
    sudo chmod 755 php5_4_downgrade_5.3.sh

然后运行这个文件： 
    
    
    sudo sh php5_4_downgrade_5.3.sh

后面的选择，如果看不懂就直接选默认吧。最后降级成功，问题解决。