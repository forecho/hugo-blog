---
title: "使用 Railway 和 Supabase 部署 Laravel 应用"
date: 2022-08-30T09:09:00+08:00
tags: ["部署", "技术分享", 'Laravel']
draft: false
toc: true
---

## 引言

[![20220830lBfWYZ](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20220830lBfWYZ.png)](https://twitter.com/novoreorx/status/1557713528609714176)

刷 Twitter 的时候看到有人分享如何使用 [Railway](https://railway.app?referralCode=HI0KtP) + [Supabase](https://supabase.com/) 部署应用，这些 PaaS 服务通常都有免费额度，如果是小项目通常相当于免费使用。

简单看了一下觉得还不错，于是我尝试部署了一份 Laravel 应用。这篇文章就来分享一下我踩过的坑。

<!--more-->

## [Railway](https://railway.app?referralCode=HI0KtP) 

Railway 提供服务器和数据库服务，注册账户就可以免费使用 500 小时（20 天），或者 5 美元以下的额度，详情看他们的 [Starter Plan](https://docs.railway.app/reference/plans#starter-plan)。

绑定信用卡之后就会按照使用情况自动扣钱了，每个月超过 5 美元就开始收费。而且没有 500 小时限制。所以 Starter Plan 版就是给你体验的，真要使用起来还是得绑定信用卡。

这里我们只使用它提供的服务器服务，数据库可以使用 Supabase 代替，以减轻 Railway 的免费额度。

## [Supabase](https://supabase.com/)

Supabase 提供 Database、Authentication、Storage、Edge Functions 服务，这里我们只需要用它的 Database 服务，免费额度有 500MB，感觉对小项目够用了。

值得注意的是 Supabase 提供 Database 是 PostgreSQL，如果要用 MySQL 的话，可以使用 [PlanetScale](https://planetscale.com/)。

## 部署 Laravel 应用

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202208301hLOM8.png)

Railway 新建项目的时候可以选 Laravel 模板，但是坑爹的是，这个模板根本无法正常使用，我摸索了半天，经过几个小时的踩坑（Google + 不断尝试）最后终于正常部署了。

其实只要在 Laravel 项目根目录加上 `Procfile` 文件就可以了，代码如下：

```
web: (cd /app && cp .env.example .env && php artisan key:generate && php artisan migrate) && ([ -e /app/storage ] && chmod -R ugo+w /app/storage); perl /assets/transform-config.pl /assets/nginx.template.conf /nginx.conf && echo "Server starting on port $PORT" && (php-fpm -y /assets/php-fpm.conf & nginx -c /nginx.conf)
```

由于我们使用的是 Supabase 数据库，所以这里我们要输入环境变量，配置从 Supabase 获取到。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202208304wQKFo.png)

Railway 支持自动化部署，自定义域名，看起来还不错。

Supabase 的使用就不讲了，很简单。

## 最后

Railway 看起来是一个很新的平台，不是很完善，目前我没找到如何部署跑队列服务和定时脚本服务，在没找到这两个解决方案之前我不打算使用 Railway。如果你没有这两个需求倒是可以试试。

说起 PaaS，就不得不提起 [Heroku](https://www.heroku.com/) ，但是它似乎在被 Salesforce 收购之后不思进取了，可惜了。希望这行业有更多的平台涌出吧，有竞争才是好事。

另外听说 [Fly.io](https://fly.io/) 也是一个不错的平台，下回有时间可以研究一下。

## 参考资料

- [使用 Railway 和 Miniflux 零成本搭建 RSS 服务](https://blog.cysi.me/2022/05/build-miniflux-rss-on-railway.html)

