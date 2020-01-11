---
title: "使用 Postman"
date: 2019-03-01T21:58:00+08:00
tags: ["restful", "工具控"] 
draft: false
toc: true
---

{% raw %}

## 引言

[上篇文章](https://blog.forecho.com/use-jwt.html)我们讲到 API 可能会使用到的 JWT 认证。既然讲到 API 开发就不得不讲到 API 调试以及测试神器 - [Postman](https://www.getpostman.com/)。

## 基本使用

### 创建环境变量

![创建环境变量](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160357.png)

根据上面截图的地方，可以找到创建环境变量的设置。设置按钮旁边的『眼睛』按钮可以查看当前使用的环境变量的值。

<!--more-->

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160522.png)

先设置环境名称，再设置环境的 `Key` 和 `Value` ，如上图设置，如果你想调用 `http` 的值只要使用 `{{http}}` 就可以调用。 

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160542.png)

一个典型的 RESTful 请求如上图所示。

### 获取请求的代码

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160601.png)

POST 还可以非常方便的获取各种语言的请求代码，方便你在开发中使用。

### 查看 URL 请求的 `Request` 和 `Response`

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160614.png)

在软件的左下角，找到终端的图标，就可以打开 Postman Console 。

### 其他

- 登录账号之后可以同步收藏夹。
- 免费用户的功能基本够用。

## 高级玩法

### Pre-request Scripts

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160627.png)

Postman 有一个 Pre-request Scripts 功能，可以在发生请求之前执行一段自定义脚本。这个功能在请求需要 Token 验证的 API，非常有用。比方说我们现在请求的接口需要 JWT 验证，传统的方式就是我们在其他地方算好 Token，粘贴过来就可以了。但是这种方式有很大的弊端，因为一般的 Token 都有实效时间的，所以 Token 实效了，下次你又得重复操作一遍。

但是有了 Pre-request Scripts 功能，我们就可以自动计算 Token，不用考虑实效问题，示例：

- 先去环境变量添加 `client_key` 和 `client_secret`
- 然后再 Pre-request Scripts 框输入下面代码：

```javascript

var removeIllegalCharacters = function(input) {
    return input
        .replace(/=/g, '')
        .replace(/\+/g, '-')
        .replace(/\//g, '_');
};

var base64object = function(input) {
    var inputWords = CryptoJS.enc.Utf8.parse(JSON.stringify(input));
    var base64 = CryptoJS.enc.Base64.stringify(inputWords);
    var output = removeIllegalCharacters(base64);
    return output;
};

var clientKey = postman.getEnvironmentVariable('client_key');
var clientSecret = postman.getEnvironmentVariable('client_secret');

var exp = Math.floor(Date.now() / 1000) + 15000;
var now = Math.floor(Date.now() / 1000);
var header = { 'alg': 'HS256', 'typ': 'JWT' };
var payload = { 'exp': exp, 'iat': now, 'client_id': clientKey};

var unsignedToken = base64object(header) + "." + base64object(payload);

var signatureHash = CryptoJS.HmacSHA256(unsignedToken, clientSecret);
var signature = CryptoJS.enc.Base64.stringify(signatureHash);
var token = 'Bearer ' + unsignedToken + '.' + signature;

postman.setGlobalVariable('jwt_token', removeIllegalCharacters(token));

```

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160640.png)

- 最后在 Header 里面配置 Authorization 就可以直接用 `{{jwt_token}}` 了


![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160651.png)

上述方式是给单独的 API 的配置 Pre-request Scripts ，Postman 还可以给集合配置 Pre-request Scripts，这样的话整个集合就可以都使用这个 `{{jwt_token}}` 了。但是值得**注意**的有：

- 集合里面配置是 Variables，单独配置的话只能设置环境变量。
- 如果集合配置了 Pre-request Script ，单独又配置了环境变量，Pre-request Script 脚本会**优先读取**环境变量的值，这个要非常注意。
- 集合的 Pre-request Scripts 获取变量方式有点不一样，比方说上面的生成 JWT Token 脚本，要使用 `pm.variables.get('client_key')` 代替 `postman.getEnvironmentVariable('client_key')`。

更多使用方法，请查看[官方文档 Pre-request scripts](https://learning.getpostman.com/docs/postman/scripts/pre_request_scripts/)。

### 内置函数

我们除了可以自定义脚本之外，Postman 还内置了一些常用的变量:


- `{{$guid}}`：v4 样式的 guid
- `{{$timestamp}}`：当前时间戳
- `{{$randomInt}}`：0到1000之间的随机整数

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424160716.png)

更多使用方法，请查看[官方文档 Variables](https://learning.getpostman.com/docs/postman/environments_and_globals/variables/)。


### Tests

目前还没用到，下次补上……

## 最后

如果你做也做接口开发，需要经常测试接口的话，推荐你使用 Postman，如果你经常使用 Postman 的话，可以多花的时间研究一下这个工具的使用方式，一定会为你之后节约很多时间。

正所谓『磨刀不误砍柴工』。

{% endraw %}