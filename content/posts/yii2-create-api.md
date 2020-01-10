---
title: "Yii2 高级版新建一个 Api 应用"
date: 2015-07-27T22:25:00+08:00
tags: ["yii2", "api"] 
draft: false
toc: true
---

先在项目的根目录下复制一份 backend 为 api：

```sh
cp backend/ api -r
```

拷贝 api 环境

```sh
cp -a environments/dev/frontend environments/dev/api
cp -a environments/prod/frontend environments/prod/api
```

修改 environments/index.php 文件之后的代码（主要是添加了一些 api 相关的代码）：

```php
return [
    'Development' => [
        'path' => 'dev',
        'setWritable' => [
            'backend/runtime',
            'backend/web/assets',
            'frontend/runtime',
            'frontend/web/assets',
            'api/runtime',
            'api/web/assets',
        ],
        'setExecutable' => [
            'yii',
        ],
        'setCookieValidationKey' => [
            'backend/config/main-local.php',
            'frontend/config/main-local.php',
            'api/config/main-local.php',
        ],
    ],
    'Production' => [
        'path' => 'prod',
        'setWritable' => [
            'backend/runtime',
            'backend/web/assets',
            'frontend/runtime',
            'frontend/web/assets',
            'api/runtime',
            'api/web/assets',
        ],
        'setExecutable' => [
            'yii',
        ],
        'setCookieValidationKey' => [
            'backend/config/main-local.php',
            'frontend/config/main-local.php',
            'api/config/main-local.php',
        ],
    ],
];
```

<!--more-->

然后再执行初始化命令：

```sh
php init
```

然后记得去 common/config/bootstrap.php 最后一行添加如下代码：

```php
Yii::setAlias('api', dirname(dirname(__DIR__)) . '/api');
```

修改一下配置文件 api/config/main.php

```php
return [
    'id' => 'app-api',
    // ...
    'controllerNamespace' => 'api\controllers',
]
```

最后 api 里面的控制器等有命名空间的文件也要修改一下。

**参考资料**

- [在原有的Yii2框架上，新建一个api应用](http://blog.phpor.me/%E5%B7%A5%E4%BD%9C/yii/2014/11/17/yii2-usage.html#create_new_app)
- [第一讲：基础配置](http://www.digpage.com/video_1.html)