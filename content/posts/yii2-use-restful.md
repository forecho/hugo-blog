---
title: "Yii2 如何实现 RESTful？"
date: 2015-03-22T20:25:00+08:00
tags: ["yii2", "restful"] 
draft: false
toc: true
---

## 什么是REST

简单的说，REST 指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是 RESTful。

## 为什么要Restful？

主要遵循两个准则：

- 不要为了RESTful而RESTful
- 在能表达清楚的情况下，简单就是美

## Yii2 使用 RESTful？

其实 Yii2 框架本身就对 RESTful 是友好支持的，具体可以看[官方文档](http://www.yiiframework.com/doc-2.0/guide-rest-quick-start.html)，或者去看源码，都是可以的。

**下面分享我写的[Yii2 RESTful DEMO](https://github.com/iiYii/yii2-rest-demo)。**

具体实现可以看我的代码，有不懂的可以留言。下面我重点说一下值得注意的地方：

<!--more-->

### Yii2 RESTful 如何实现自定义方法？


新建一个 user 控制器，输入以下代码：

```
namespace app\controllers;

use yii\rest\ActiveController;

class UserController extends ActiveController
{
    public $modelClass = 'app\models\User';
}
```

然后你在浏览器里面输入 用 GET 请求 `localhost/basic/web/users` 这个地址是可以跑的，因为继承的 `ActiveController` 这个类是有对 user RESTful 表的 基本操作的，包括 GET，POST，PUT，PATCH，DELETE。所以我们要注销系统自带的实现方法，代码如下，添加到控制器中，后者自己新建一个 ActiveController 类。

```
public function actions()
{
   $actions = parent::actions();
    // 注销系统自带的实现方法
    unset($actions['index'], $actions['update'], $actions['create'], $actions['delete'], $actions['view']);
    return $actions;
}
```

然后下面我们就可以写自己的 actionIndex、actionCreate、actionUpdate 和 actionDelete 了。

### 实现用户验证

框架其实是提供三种验证方式的，我就说一下最长用的 token 实现验证吧。

首先你要在控制器中加入下面代码：

```
public function behaviors()
{
    $behaviors = parent::behaviors();
    $behaviors['authenticator'] = [
        'class' => CompositeAuth::className(),
        'authMethods' => [
            QueryParamAuth::className(),
        ],
    ];
    return $behaviors;
}
```

然后你要去修改 user 的 model，具体参照这块代码：[添加 restful 授权认证](https://github.com/iiYii/yii2-rest-demo/commit/02db1711bcaa42040360d50ffcf771626474f5ad)


## 参考文章

- [理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)
- [RESTful实践总结](http://segmentfault.com/blog/cloudmario/1190000000635914)