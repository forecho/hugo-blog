---
title: "Laravel 使用远程 API 作为认证"
date: 2021-12-24T15:28:00+08:00
tags: ["laravel", "api", "auth"]
draft: false
toc: true
---

## 引言

最近做系统拆分的工作，先把用户模块拆出来，做了一个 Account-System，使用 Laravel 的 Passport 来做认证，方便快捷。

然后把钱包功能也拆出来了，做了一个 Wallet-System，用户认证都去请求 Account-System，钱包相关都去请求 Wallet-System。这篇文章主要是分享怎么让 Laravel 使用远程 API 作为认证。

## 流程

两个系统的职责不一样，所以还是先简单介绍一下流程：

- 用户登录和注册都是去 Account-System 进行的。
- 用户登录成功之后返回一个 Access-Token。
- 访问 Wallet-System 的时候，需要带上 Access-Token。
- Account-System 提供一个验证 Access-Token 的 API。Wallet-System 在处理请求之前先去 Account-System 验证 Access-Token。

<!--more-->

## 实践

下面就来分享 Wallet-System 如何使用远程 API 作为认证。

### 自定义 Auth Guard

修改 `config/auth.php`：


```php
<?php

return [

    'defaults' => [
        'guard' => 'web',
    ],


    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
    ],

    'providers' => [
        'users' => [
            'driver' => 'custom',
        ],
    ],

];

```

修改 `app/Providers/AuthServiceProvider.php`，添加 `provider` 方法：

```php
<?php

namespace App\Providers;

use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;
use Illuminate\Support\Facades\Auth as BaseAuth;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * Register any authentication / authorization services.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerPolicies();
        $this->provider();
    }

    /**
     * @return void
     */
    protected function provider(): void
    {
        BaseAuth::provider('custom', static function (): CustomUserProvider {
            return new CustomUserProvider();
        });
    }
}

```

添加 `app/Providers/CustomUserProvider.php` 文件：


```php
<?php

namespace App\Providers;

use Illuminate\Contracts\Auth\Authenticatable;
use Illuminate\Contracts\Auth\UserProvider;

class CustomUserProvider implements UserProvider
{
    public function retrieveById($identifier)
    {
        // TODO: Implement retrieveById() method.
    }

    public function retrieveByToken($identifier, $token)
    {
        // TODO: Implement retrieveByToken() method.
    }

    public function updateRememberToken(Authenticatable $user, $token)
    {
        // TODO: Implement updateRememberToken() method.
    }

    public function retrieveByCredentials(array $credentials)
    {
        // TODO: Implement retrieveByCredentials() method.
    }

    public function validateCredentials(Authenticatable $user, array $credentials)
    {
        // TODO: Implement validateCredentials() method.
    }
}
```

### 添加验证方法


添加 `app/Services/AuthService.php` 文件：

```php
<?php

declare(strict_types=1);

namespace App\Services;

use App\Models\User;
use App\Services\ThirdParty\AccountServiceService;
use Illuminate\Support\Facades\Auth as BaseAuth;

class AuthService
{
    /**
     * @var AccountServiceService
     */
    private $accountService;

    public function __construct(AccountServiceService $accountService)
    {
        $this->accountService = $accountService;
    }

    /**
     * @param  string  $token
     * @return User
     */
    public function byCredentials(string $token): User
    {
        $user = $this->accountService->getUserByToken($token);
        $user['token'] = ltrim($token, 'Bearer ');

        return static::auth(new User($user));
    }

    /**
     * @param  User  $user
     *
     * @return User
     */
    protected static function auth(User $user): User
    {
        static::session($user);
        static::bind($user);
        static::login($user);

        return $user;
    }

    /**
     * @param  User  $user
     *
     * @return void
     */
    protected static function session(User $user): void
    {
        session(['token' => $user->token]);
    }

    /**
     * @param  User  $user
     *
     * @return void
     */
    protected static function bind(User $user): void
    {
        app()->bind('user', static function () use ($user): User {
            return $user;
        });

        app()->bind('token', static function (): string {
            return session('token');
        });
    }

    /**
     * @param  User  $user
     *
     * @return void
     */
    protected static function login(User $user): void
    {
        BaseAuth::login($user, true);
    }
}
```

上面的 `getUserByToken` 我就不贴了，这个方法就是请求 Account-System 验证 Access-Token 的 API，根据结果转化为 `User` 对象，这个对象包含了用户的信息。

然后修改 `app/Models/User.php` 文件：

```php
<?php

namespace App\Models;

use Illuminate\Auth\Authenticatable;
use Illuminate\Contracts\Auth\Authenticatable as AuthenticatableContract;

/**
 * @property string $token
 */
class User implements AuthenticatableContract
{
    use Authenticatable;

    /**
     * @return string
     */
    public function getAuthIdentifierName(): string
    {
        return 'token';
    }

    /**
     * @return string
     */
    public function getAuthIdentifier(): string
    {
        return $this->token;
    }
}
```

### 中间件

因为 Wallet-System 每个接口都是要验证 Access-Token 的，所以我们需要添加一个中间件，来验证 Access-Token 的有效性。

添加 `app/Http/Middleware/CustomAuth.php` 文件：

```php
<?php

namespace App\Http\Middleware;

use App\Services\AuthService;
use Closure;
use Illuminate\Auth\AuthenticationException;
use Illuminate\Http\Request;

class CustomAuth
{
    /**
     * @var AuthService
     */
    private $authService;

    public function __construct(AuthService $authService)
    {
        $this->authService = $authService;
    }

    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure(\Illuminate\Http\Request): (\Illuminate\Http\Response|\Illuminate\Http\RedirectResponse)  $next
     * @return \Illuminate\Http\JsonResponse
     * @throws AuthenticationException
     */
    public function handle(Request $request, Closure $next)
    {
        if (!$request->header('Authorization')) {
            throw new AuthenticationException('No authorization header was found');
        }
        $this->authService->byCredentials($request->header('Authorization', ''));

        return $next($request);
    }
}
```

使用中间件，修改 `app/Http/Kernel.php` 文件：

```php
 protected $middlewareGroups = [
    // something code
    'api' => [
        \App\Http\Middleware\CustomAuth::class,
        // something code
    ],
];
```

## 总结

至此所有步骤都完成了。总结下来主要是这三步：

- 自定义 Auth Guard
- 中间件去验证 Access-Token 并且 Set Auth
- 修改 `User` Model

## 参考文档

- [《Laravel Auth and Session without database. Using a remote API as Auth and Data provider.》](https://gist.github.com/eusonlito/8b5389db1d390c17aba123645fd99ea1)