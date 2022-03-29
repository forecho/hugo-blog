---
title: "Laravel Passport OAuth2 实践"
date: 2022-03-28T17:30:00+08:00
tags: ["laravel", "oauth2", "passport"] 
draft: false
toc: true
---

## 引言

最近在搞 OAuth2 Service 相关功能，踩了很多坑，打算记录分享一下。

这篇文章主要讲 OAuth2 的实现，以及 [Laravel Passport](https://laravel.com/docs/9.x/passport) 的使用。

## 介绍 OAuth2

简单来讲 OAuth 2.0 就是一个行业的标准授权协议。目的是为了给第三方应用颁发一个有时效性的访问令牌，以便第三方应用能够访问被授权的资源，我们常见场景有第三方登录。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20220328fYuKxr.jpg)

<!--more-->

### 流程

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202203282ECUEh.png)

我们以 [miro](https://miro.com/) 网站为例，简单记录了一下使用流程：

1. 用户使用 Facebook 联合登录。
2. 如果之后用户未登录 Facebook，则网站跳转到 Facebook 登录页面
3. Facebook 询问用户是否同意授权其在 Facebook 的资料，一般都是 email 和名字。
4. 用户点击同意之后，miro 可以获取用户 Facebook 的资料。

### 角色

- 资源所有者（Resource Owner）：拥有资源的人，比如登录 Facebook 的人，通常是你。
- 资源服务器（Resource Server）：拥有受保护资源的服务器，比如 Facebook 就是资源服务器，它拥有你的用户资料。
- 授权服务器（Authorization Server）：授权服务器，用来验证用户身份然后为客户端派发令牌，上述示例中 Facebook 即是资源服务器也是授权服务器。实际情况是不同的服务往往会拆分。
- 客户端（Client）：通常是指想要获取受到保护资源的应用，上述示例中 miro 就是客户端。

### 名词解释

- Authorization Grant : 授权许可，指用户同意授权给客户端，客户端可以获取一定范围内的资源。
- Redirect URI（Callback URL）：授权成功之后的回调地址，客户端可以拿到这个回调地址，然后跳转到这个地址。
- Access Token ：授权成功之后，客户端可以拿到的访问令牌，用它去访问资源服务器的资源。
- Scope：授权范围，指客户端可以获取的资源范围。通常是一个清单，比如我们可以获取用户的 email 和名字，可以编辑用户的资料等等。

### 协议流程

![OAuth2 Abstract Flow](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20220329XRMehI.jpeg)

OAuth2 的主要授权流程是：

1. 客户端请求资源所有者授权
2. 资源所有者授权给客户端
3. 客户端向授权服务器申请访问令牌
4. 授权服务器授权给客户端有效的访问令牌
5. 客户端使用访问令牌访问资源服务器请求资源
6. 资源服务器验证令牌并返回资源

### 授权类型

OAuth2 有四种授权方式获取资源：

- Authorization Code（最常用）：授权码模式，客户端每次请求资源服务器都需要提供授权码，授权码是客户端授权给资源服务器的一个访问令牌，它可以访问资源服务器的资源。
- Implicit：隐式授权，客户端可以不提供授权码，直接访问资源服务器的资源。用于简化授权流程，缺少「使用 Authorization Code Grant 交换 Access Token」这一步。
- Resource Owner Password Credentials：密码模式，客户端通过用户名和密码获取资源。用户输入账号密码登录，这种模式适合高度信任的应用程序，比如说内部系统。
- Client Credentials：客户端模式，客户端可以不提供用户名和密码，直接获取资源。跳过用户授权的步骤。

## Laravel 使用 OAuth2 Server

### 搭建

Laravel 官方提供了一个组件 [Laravel Passport](https://laravel.com/docs/9.x/passport)，可以让你快速的搭建一个标准的 OAuth2 服务。

使用方式这里就多说了，多看看官方文档就可以了。

### API 使用方式

这才是本篇文章想要分享的重点。官方文档的使用说明没有提到如何通过 API 的方式使用，但是目前前后端分离的开发方式已经成为主流了，所以我就自己去摸索了，网上能找到的资料也很少，基本是没找到。

**配置参数**

修改 `config/auth.php` 文件，修改之后的配置：

```php
<?php

return [
    'defaults' => [
        'guard' => 'api',
        'passwords' => 'users',
    ],
    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'api' => [
            'driver' => 'passport',
            'provider' => 'users',
        ],
    ],
    'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\Models\User::class,
        ],
    ],
    'passwords' => [
        'users' => [
            'provider' => 'users',
            'table' => 'password_resets',
            'expire' => 60,
            'throttle' => 60,
        ],
    ],
    'password_timeout' => 10800,
],

```

重点说明：

- 把默认的 `guard` 改成 `api` 了
- `guards.api.driver` 改成 `passport`


**配置路由**

修改 `app/Providers/AuthServiceProvider.php` 文件，修改之后的配置：

```php
<?php

namespace App\Providers;

use App\Enums\Scope;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
use Laravel\Passport\Passport;

class AuthServiceProvider extends ServiceProvider
{
    protected $policies = [
        'App\Models\Model' => 'App\Policies\ModelPolicy',
    ];

    public function boot()
    {
        $this->registerPolicies();
        if (!$this->app->routesAreCached()) {
            Passport::ignoreCsrfToken();
            Passport::routes(null, ['prefix' => 'api/v1/oauth', 'middleware' => ['api']]);
        }

        Passport::tokensCan(Scope::descriptions());
    }
}

```

重点说明：

- 这里我有改动默认的路由，为了统一我的 API 路由风格，我把 `/oauth` 改成 `/api/v1/oauth`

**使用 Openid**

如果你也有「给不同的 Client 分配不同的 Openid 」的需求的话，可以看看这个 - [《laravel passport 加密 jwt 格式的 access_token 中的 sub(user_id) 字段》](https://baijunyao.com/articles/189)

结合上面的文章，使用 `hashids`加密的时候加上 `client_id` 就可以实现了以上需求了。


## 最后

OAuth2 概念其实挺多的，但是其应用也是非常广泛的。之所有搞这么复杂还是为了安全性。不同的使用场景选择不同的授权模式。

另外我也找到了一个在线体验 OAuth2 的网站 - [OAuth 2.0 Playground](https://www.oauth.com/playground/) 可以方便你加速理解。

## 资料

- [[筆記] 認識 OAuth 2.0：一次了解各角色、各類型流程的差異](https://medium.com/%E9%BA%A5%E5%85%8B%E7%9A%84%E5%8D%8A%E8%B7%AF%E5%87%BA%E5%AE%B6%E7%AD%86%E8%A8%98/%E7%AD%86%E8%A8%98-%E8%AA%8D%E8%AD%98-oauth-2-0-%E4%B8%80%E6%AC%A1%E4%BA%86%E8%A7%A3%E5%90%84%E8%A7%92%E8%89%B2-%E5%90%84%E9%A1%9E%E5%9E%8B%E6%B5%81%E7%A8%8B%E7%9A%84%E5%B7%AE%E7%95%B0-c42da83a6015)
- [OAuth 2 深入介绍 ](https://www.cnblogs.com/Wddpct/p/8976480.html)
- [10 分钟理解什么是 OAuth 2.0 协议](https://deepzz.com/post/what-is-oauth2-protocol.html)