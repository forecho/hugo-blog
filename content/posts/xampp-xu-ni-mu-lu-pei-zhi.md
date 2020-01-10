---
title: "XAMPP 虚拟目录配置"
date: 2011-11-21T09:36:00+08:00
tags: ["XAMPP", "php"] 
draft: false
toc: true
---

我下载的是 xampp 1.77版本的。 安装的时候一直点下一步，不用有任何操作。 打开D:\xampp\apache\conf\extra\httpd-vhosts.conf 这个文件 把

```
NameVirtualHost *:80
```

前面的 ## 注释去掉。这个是开启虚拟目录的设置。 然后在最下面加上

```
<VirtualHost *:80>
DocumentRoot "D:/xampp/htdocs"
ServerName 127.0.0.1
</VirtualHost>

<VirtualHost *:80>
DocumentRoot "F:/workspace"
ServerName localhost
<Directory "F:/workspace">
Options Indexes FollowSymLinks Includes ExecCGI
AllowOverride All
Order allow,deny
Allow from all
</Directory>
</VirtualHost>
```

其中的路径你要根据你的路径具体情况而定。第二个就是配置虚拟路径的方法。[参考文件](http://www.yyduo.com/84)