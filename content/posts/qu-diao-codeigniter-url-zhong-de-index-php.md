---
title: "去掉CodeIgniter URL中的index.php"
date: 2011-11-14T17:30:00+08:00
tags: ["CodeIgniter"] 
---

CI默认的rewrite url中是类似这样的，例如你的CI根目录是在/CodeIgniter/下，你的下面的二级url就类似这样http://localhost/CodeIgniter/index.php/welcome。不太好看，怎么把其中的index.php取掉呢？

- 打开apache的配置文件，conf/httpd.conf ：

```
LoadModule rewrite_module modules/mod_rewrite.so
```

把该行前的#去掉。

搜索 AllowOverride None（配置文件中有多处），看注释信息，将相关.htaccess的该行信息改为AllowOverride All。

- 在CI的根目录下，即在index.php，system的同级目录下，建立.htaccess，直接建立该文件名的不会成功，可以先建立记事本文件，另存为该名的文件即可。内容如下（CI手册上也有介绍）：

```
RewriteEngine on
RewriteCond $1 !^(index\.php|images|robots\.txt)
RewriteRule ^(.*)$ /index.php/$1 [L]
```

如果文件不是在www的根目录下，例如我的是：http://www.nowamagic.net/CodeIgniter/，第三行需要改写为

```
RewriteRule ^(.*)$ /CodeIgniter/index.php/$1 [L]
```

另外，我的index.php的同级目录下还有js文件夹和css文件夹，这些需要过滤除去，第二行需要改写为：

```
RewriteCond $1 !^(index\\.php|images|js|css|robots\\.txt)
```

- 将CI中配置文件（system/application/config/config.php）中$config['index_page'] = "index.php";改成$config['index_page'] = ""; 。

```
/*
|--------------------------------------------------------------------------
| Index File
|-----------------------------------------------------------------------draft: false
toc: true
---
|
| Typically this will be your index.php file, unless you've renamed it to
| something else. If you are using mod_rewrite to remove the page set this
| variable so that it is blank.
|
*/
$config['index_page'] = '';
```

ok，完成。还要记得重启apache。 就这么简单，好好体验CI吧～