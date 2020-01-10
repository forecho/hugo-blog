---
title: "Laravel 写单元测试"
date: 2019-01-14T15:03:00+08:00
tags: ["laravel"] 
draft: false
toc: true
---


## 引言

新公司入职第一件事就是让我给一个用 Laravel 框架开发的项目写单元测试，我会说『当了这么多年的程序员没写过单元测试吗？』，于是我一边学习一边写，这篇文章是我写了一个月单元测试的经验分享。

## 单元测试

### 基本的断言


- 断言两边数据相等使用 `assertEquals($a, $a1)`

```
public function testComputePermission()
{
    $totalPermission = TCH::computePermission([PointsAccountPermissions::TRANSFER]);
    $this->assertEquals($totalPermission, PointsAccountPermissions::TRANSFER);
}
```

- 断言结果是 `true` 使用 `assertTrue($a)`，与之相反的是 `assertFalse($b)`
- 断言文件是否存在使用 `assertFileExists($path)`
- 断言数据库没有数据使用 `assertDatabaseMissing('user', [])`
- 断言两个类一样使用 `assertSame($a, $b)`
- ……

<!--more-->

### 断言返回数据数组包含某个键值

```php
/**
* @throws \App\Exceptions\ThirdPartyServiceErrorException
*/
public function testDecodeUnionId()
{
    $accountBaseService = app()->make(AccountBaseService::class);
    $unionId = mt_rand(1, 100);
    $response = $accountBaseService->decodeUnionId([$unionId]);
    $this->assertArrayHasKey('data', $response);
    $data = data_get($response, 'data.list.0');
    $this->assertArrayHasKey('brand_id', $data);
    $this->assertArrayHasKey('account_id', $data);
}
```

### 多组数据测试

多组数据可以使用 `@dataProvider` 的方式传入数据来测试。比方说我测试一个金额，只支持正数、两位小数。

```php
namespace App\Rules\Rules;

use App\Rules\AmountRule;
use PHPUnit\Framework\TestCase;

class AmountRuleTest extends TestCase
{
    /**
     * @var AmountRule()
     */
    protected $rule;

    public function setUp()
    {
        parent::setUp();
        $this->rule = new AmountRule();
    }

    /**
     *
     * @dataProvider evenPassData
     * @param int|float $int
     * @return void
     */
    public function testEvenPass($int)
    {
        $this->assertTrue($this->rule->passes('test', $int));
    }

    /**
     *
     * @dataProvider evenFailData
     * @param int|float $int
     * @return void
     */
    public function testEvenFail($int)
    {
        $this->assertFalse($this->rule->passes('test', $int));
    }

    public function evenPassData()
    {
        return [
            [100],
            [100.11],
            [2],
            [2.00],
            [2.01],
            [2.1],
        ];
    }

    public function evenFailData()
    {
        return [
            [-1],
            [1.111],
            [-1.1],
            [0],
        ];
    }
}
```

### Console 测试

```php
namespace App\Console\Commands;

use App\Models\ClientApp;
use Illuminate\Console\Command;
use Webpatser\Uuid\Uuid;

class CreateClient extends Command
{
    //  something code

    /**
     * Execute the console command.
     *
     * @return mixed
     * @throws \Exception
     */
    public function handle()
    {
        $title = $this->ask('客户端应用名称');
        $desc = $this->ask('客户端应用描述');
        $access = $this->ask('access (1: 私有  2: 内部　３:　公开)');

        $appId = Uuid::generate();
        $secret = str_random(64);

        $data = [
            // something code
        ];

        ClientApp::create($data);

        $this->info("Done!\n");
        $this->info('client_id: ' . $appId);
        $this->info('secret: ' . $secret);
    }
}

```


```php
public function testCreateClient()
{
    $application = new Application();

    $testedCommand = $this->app->make(CreateClient::class);
    $testedCommand->setLaravel(app());
    $application->add($testedCommand);

    $commandTester = new CommandTester($testedCommand);
    $commandTester->setInputs(['测试', '单元测试', 1]);
    $commandTester->execute(['command' => $testedCommand->getName()]);

    $this->assertRegExp('/client_id: \w{8}(-\w{4}){3}-\w{12}\\n/', $commandTester->getDisplay());
    $this->assertDatabaseHas(ClientApp::TABLE_NAME, [
        'title' => '测试',
        'description' => '单元测试',
        'access' => 1,
    ]);
}
```

### 断言类

```php
/**
* @test
*/
public function getAccountByUnionId()
{
    $service = new PointsAccountService();
    $record = $service->getAccountByUnionId(1);
    $this->assertInstanceOf(PointsAccount::class, $record);
}
```

> PS: 方法含有 test 或者使用注释 `@test` 都可以识别是单元测试的一个方法。

### 异常测试

默认没有测试异常的方法，所以我们自己实现一个。

```php
/**
* Asserts that the given callback throws the given exception.
*
* @param string $expectClass The name of the expected exception class
* @param callable $callback A callback which should throw the exception
*/
protected function assertException(string $expectClass, callable $callback)
{
    try {
        $callback();
    } catch (\Throwable $exception) {
        $this->assertInstanceOf($expectClass, $exception, 'An invalid exception was thrown');
        return;
    }

    $this->fail('No exception was thrown');
}
```

```php
public function testSomeException()
{
    $service = new PointsAccountService();
    $this->assertException(\InvalidArgumentException::class, function () use ($service) {
        $service->getAccountByUnionId(0);
    });
}
```

### 队列测试


```php
use Illuminate\Support\Facades\Queue;

public function testJob()
{
    Queue::fake();

    // 推送数据到队列
    SomeJob::dispatch($vars);

    // 断言已经在队列中
    Queue::assertPushed(SomeJob::class);
}
```

需要补充的是队列这块注意 `phpunit.xml` 文件里面的 `<env name="QUEUE_CONNECTION" value="sync"/>` 代码，意思是使用同步的方式跑队列。这里要根据自己的情况修改配置。

### API 接口测试

```php
use Illuminate\Http\Request;
public function testUserCreate()
{
    $data = [
        'username' => 'test',
    ];
    $response = $this->json(Request::METHOD_POST, config('app.url') . '/users', $data);
    $response->assertStatus(200)
        ->assertJson([
            'code' => 0,
        ]);
    // something other assert
}
```

### 第三方接口依赖测试

如果我们项目中有依赖第三方接口，我们测试的时候不会直接去请求第三方接口，而是断言其接口成功返回。那么具体怎么测试呢？使用 mock 是一种方式，但是我使用的是另外一种方式：

- 添加接口返回的数据，比方说添加 `tests/fixtures/simple_data/vendor/v1/member/info.json` 文件，里面就贴上接口正常返回的数据。
- 添加控制器方法 `app/Http/Controllers/TestController.php` 文件，里面的方法做文件映射：

```php
namespace App\Http\Controllers;

use Illuminate\Http\Response;
use Illuminate\Routing\Route;

class TestController extends Controller
{
    public function index(Route $route): Response
    {
        if ($url = str_replace('api/tests/v1', '', $route->uri)) {
            $path = base_path('/tests/fixtures/simple_data/vendor/v1');
            $data = file_get_contents("{$path}{$url}.json");
            return response($data, 200, ['Content-Type' => 'application/json']);
        }
        return response('');
    }
}
```

- 添加路由配置文件 `routes/api.php`

```php
// tests v1 版本 account-base-service 和 notification
Route::group(['prefix' => 'tests/v1',], function () {
    Route::get('member/info', 'TestController@index');
    Route::post('member/create', 'TestController@index');
    // ……
});
```

> PS: 这里最后两步之所以不使用路由回调函数方式实现，是因为 `php artisan route:cache` 命令不支持路由 `Closure`

### 关于清除测试数据


Laravel 自带一个 `RefreshDatabase` trait，如果你的测试类加入了 `use RefreshDatabase;` 代码，那么每跑一个测试会清除数据。但是我嫌这样跑测试太慢，基本上都不用这个方法。

Laravel 默认如果有 `.env.testing` 环境配置文件的话，跑单元测试会使用这个文件的配置，里面可以配置单独的数据库等其他配置。所以我目前本地使用的是这种方式来跑单元测试。

### 加速跑单元测试

跑单元测试我们一般用下面这个命令跑：

```sh
vendor/bin/phpunit
```

但是单元测试多了之后，上面这种方式跑单元测试会变慢，我们可以用 [phpdbg](https://github.com/krakjoe/phpdbg) 来加速跑运行单元测试，以节省时间。

安装：

```
sudo apt-get install php7.2-phpdbg -y
```

使用：

```
phpdbg -qrr ./vendor/bin/phpunit -dmemory_limit=1024M  
```

如果还是嫌慢，还有优化空间，以后再补充。

## 最后

我们的单元测试只在本地和 [travis](https://travis-ci.org/) 里面跑，自从写了单元测试之后，改代码要放心的很多。如果时间允许的话，推荐大家也多写写单元测试。养成好习惯。