---
title: "Ubuntu安装PHP Mongo扩展"
date: 2013-07-26T15:58:00+08:00
categories: 
draft: false
toc: true
---

如果你没有安装PHP的PECL，就先在终端执行下面命令： 
    
    
    sudo apt-get install php5-dev php5-cli php-pear

然后安装命令： 
    
    
    sudo pecl install mongo

找到你的php.ini配置文件，打开（下面是我系统的php.ini文件路径，你需要找到你的配置文件）： 
    
    
    vim /etc/php5/fpm/php.ini

手动添加下面这行代码： 
    
    
    extension=mongo.so

重启一下PHP： 
    
    
    sudo service php5-fpm restart

然后查看你的info.php文件（查看PHP配置文件），如果你能搜索到“mongo”，那就说明你安装成功了。