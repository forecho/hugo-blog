---
title: "使用 JWT"
date: 2019-02-25T21:58:00+08:00
tags: ["restful"] 
draft: false
toc: true
toc: false       # 关闭文章目录
reward: false	 # 关闭打赏
---


## 引言

API 开发一定会涉及到认证问题，本篇文章就是结合我自己工作经验来分享一下我用过的 JWT 认证以及它和普通认证的区别，希望本篇文章对你在开发 API 认证时有用。

## 什么是 JWT

[JWT](https://jwt.io/) 全名是 JSON Web Tokens，是一个基于 JSON 的开放标准用于创建声明一些声明的访问令牌。JWT 由以下三部分组成：

<!--more-->

- `header`
- `payload`
- `signature`

这三部分通过指定的算法，会生成一个类似 `xxx.yyy.zzz` 结构的字符串。

### Hearder

通常是由算法名称和 Token 类型组成的一个 JSON，用得最多的就是这个:

```json
{
   "alg": "HS256",
   "typ": "JWT"
}
```

### Payload

这部分算是 JWT 最主要的部分，这部分内容就是你需要传的数据，也是一个 JSON，内容由自定义部分和规范定义部分组成。比方说：

```json
{
    "iss": "JWT-Rails-Server", // 签发者
    "aud": "www.baidu.com", // 接收者
    "iat": 1472263256, // JWT 签发的时间
    "exp": 1472522525, // 过期时间
    "sub": "jwt@baidu.com", // JWT对应的用户 
    "user_id": 1211 // 自定义
}
```

规范定义也是可选的，一般用的最多的是 `iat` 和 `exp`。

### Signature

第三部分就是签名，它是由 `Hearder` 和 `Payload` 使用 `.` 连接成的字符串，再使用我们自己提供的一个密钥进行指定算法（比方说 HS256）加密后的字符串。

## JWT 特性

### 防止被篡改 && 信息不加密

**JWT 主要是用来防止信息被篡改**。它传递的信息不能被篡改，并不是加密的。我们可以使用 [JWT-Decode](http://calebb.net/) 工具来做个示范：

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImp0aSI6ImI4OWJmMmNiLTI0MmMtNDI1Yi05MmNmLTQyZGQyMDMyNjBiMSIsImlhdCI6MTU1MTA4Nzg3MCwiZXhwIjoxNTUxMDkxNDcwfQ.tI9NtLAuoUxLRf64H7zwAdjZKY83iZcAE_9qpcdWBXc
```

以上 JWT 会被解析为：

```
{
 typ: "JWT",
 alg: "HS256"
}.
{
 sub: "1234567890",
 name: "John Doe",
 admin: true,
 jti: "b89bf2cb-242c-425b-92cf-42dd203260b1",
 iat: 1551087870,
 exp: 1551091470
}.
[signature]
```

所以 JWT 传递的信息其实是可以被解密的，**请不要在 JWT 里面传递一些敏感的数据。**

### 减少数据库的压力

API 是根据请求过来的 JWT，进行实时解密，并且校验的。所以 JWT 可以不存入数据库当中，除非你想要实现『让某个 JWT 失效』的需求。

## 更加安全的 JWT

对称加密算法（主要基于 HMAC，如 HS256）分发 JWT 的过程是使用同一个密钥（secret）生成和验证 JWT。这种方式严格依赖秘钥，一旦秘钥被泄漏，就可以生成『假的』 JWT。

在多个团队开发过程中，JWT 的认证服务和资源服务很可能是不同的团队开发和维护，密钥在这个过程中传递，很有可能泄漏。这个时候我们应该要使用非对称加密算法（主要是基于 [RSA](https://zh.wikipedia.org/wiki/RSA%E5%8A%A0%E5%AF%86%E6%BC%94%E7%AE%97%E6%B3%95)，如 RS256）生成 JWT，即认证服务器使用私钥生成 JWT，资源服务器使用公钥去校验 JWT，认证服务去管理私钥，公钥开放给各个资源服务，这样密钥泄漏的可能性就大大降低了。

这种方式使用上类似，只是把 Header 里面的 `alg` 换成 `RS256`，生成的时候使用秘钥，解密的时候使用公钥。需要注意的是：

- 使用非对称加密算法生成 JWT 在解密的时候，不应该直接使用传过来 JWT 解密出 Header 里面的 `alg` 值，不然如果对方传过来的是 `HS256`，那他就可以随意篡改了。


## UUID 当 Token

除了 JWT 当 Token，我们还可以使用服务器生成的 UUID 当 token，当用户请求登录接口的时候，生成 UUID，并且把这个 UUID 和这个用户 ID 绑定存在数据库或者 redis 里面。当然我们还可以设置这个 Token 的过期时间。

当用户请求需要认证的 API 时候，必须要带上 Token，API 服务收到请求的时候先从存储服务里面读取 Token 以及验证 Token 是否有效。

相比 JWT，这种方式必须要存储 Token。

## 实战

了解 JWT 之后，想用 JWT 非常简单，[官网](https://jwt.io/)已经提供了常用开发语言的 JWT 相关类库，直接用就可以了。比方说 PHP 就用 [firebase/php-jwt](https://github.com/firebase/php-jwt) 。

**值得注意**

当使用非对称加密也就是 RS256 算法加密的时候，从证书中获取公钥或者私钥的时候要用 [`openssl_pkey_get_public()`](http://php.net/manual/zh/function.openssl-pkey-get-public.php) 和 [`openssl_pkey_get_private()`](http://php.net/manual/zh/function.openssl-pkey-get-private.php) 方法。

## 总结

本篇文章主要介绍了 JWT 这种无状态 Token 的特性：

- 防止被篡改 && 信息不加密
- 可以不访问数据库进行验证，高效

提高 JWT 安全的方式有：

- 通过设置有效时间，即 `exp` 参数
- 如果有必要，可以使用非对称加密方式实现生成 JWT
- 不要信任客户端传过来 JWT 里的 `alg`

最后还说了 JWT 对称加密（`HS256`）和非对称加密（`RS256`）的使用方式。

## 参考链接

- [JWT的认识与攻击](https://www.freebuf.com/column/170359.html)
- [JWT 简介（译）](http://blog.qiji.tech/archives/1723)
- [JSON Web Token - 在Web应用间安全地传递信息](http://blog.leapoahead.com/2015/09/06/understanding-jwt/)