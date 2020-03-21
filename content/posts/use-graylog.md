---
title: "使用 Graylog 收集日志"
date: 2020-03-21T20:20:26+08:00
tags: ["经验分享", '编程'] 
draft: false
toc: true
---

## 引言

随着项目越来越负责，Bug 越来越难定位，一个专门的日志收集工具必不可少。于是花了两天时间玩了一下 Graylog，这篇文章就分享一下踩过的坑。

## 准备环境

自从用过 Docker 之后，我现在搭建环境第一步就去找 Docker 安装方式，其实他们官方文档写的非常清楚，但是我还是踩了一些坑，主要是网络问题，太慢，必须要换中国源。

<!--more-->

### 安装 Docker 和 Docker compose

```
curl -sSL https://get.daocloud.io/docker | sh

curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### 换中国源

```
vim /etc/docker/daemon.json
```

添加代码：

```json
{
    "registry-mirrors": [
        "https://dockerhub.azk8s.cn",
        "https://docker.mirrors.ustc.edu.cn"
    ]
}
```

重启服务（重启才生效）

```
sudo systemctl daemon-reload && sudo systemctl restart docker
```

## 安装 Graylog

下载代码：

```
git clone https://github.com/forecho/graylog-docker.git
cd graylog-docker
```

然后修改 `docker-compose.yml` 里的环境变量：


- **`GRAYLOG_PASSWORD_SECRET`**: 密码密钥，至少16位数
- **`GRAYLOG_ROOT_PASSWORD_SHA2`**: 加密过的密码，这里默认是 `admin`，如果要修改则执行下面命令，把生成的结果复制到这里。

```shell
echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
```
- **`GRAYLOG_HTTP_EXTERNAL_URI`**: 本地部署就不需要修改，如果是服务器部署，要改成服务器 IP。

然后执行下面命令开始安装：

```shell
sudo docker-compose up -d
```

浏览器使用输入 `GRAYLOG_HTTP_EXTERNAL_URI` 环境变量的值就可以访问了，本地搭建默认是 <http://127.0.0.1:9000/>


## 测试

本地测试写入日志命令，根据需要修改最后的 IP 地址：

```shell
echo '{"version": "1.1","host":"example.org","short_message":"A short message that helps you identify what is going on","full_message":"Backtrace here\n\nmore stuff","level":1,"_user_id":9001,"_some_info":"foo","_some_env_var":"bar"}' | gzip | nc -u -w 1 127.0.0.1 12201
```

详情参考 [forecho/graylog-docker](https://github.com/forecho/graylog-docker)

## 在 Yii2 中使用

### 安装扩展包：

```shell
composer require --prefer-dist yiier/yii2-graylog-target "*"
```

### 配置

然后修改配置文件：

```php
return [
    'components' => [
        'log' => [
            'targets' => [
                'graylog' => [
                    'class' => yiier\graylog\Target::class,
                    'levels' => ['error', 'warning', 'info'],
                    // 'categories' => ['application', 'graylog'],
                    // 'logVars' => ['_GET', '_POST', '_FILES', '_COOKIE', '_SESSION'],
                    // 'facility' => 'facility-name',
                    'transport' => [
                        'class' => yiier\graylog\transport\UdpTransport::class,
                        'host' => '127.0.0.1',
                        'port' => '1231',
                        'chunkSize' => 4321,
                    ],
                    'additionalFields' => [
                        'user-ip' => function ($yii) {
                            return ($yii instanceof \yii\console\Application) ? '' : $yii->request->userIP;
                        },
                        'tag' => 'tag-name'
                    ],
                ],
            ],
        ],
    ],
];
```

### 使用

有两种方式：

- 方式一：使用 Yii 自带的日志方式：

```php
<?php
// short_message will contain string representation of ['test1' => 123, 'test2' => 456],
// no full_message will be sent
Yii::info([
    'test1' => 123,
    'test2' => 456,
]);

// short_message will contain 'Test short message',
// two additional fields will be sent,
// full_message will contain all other stuff without 'short' and 'additional':
// string representation of ['test1' => 123, 'test2' => 456]
Yii::info([
    'test1' => 123,
    'test2' => 456,
    'short' => 'Test short message',
    'additional' => [
        'additional1' => 'abc',
        'additional2' => 'def',
    ],
]);
 
// short_message will contain 'Test short message',
// two additional fields will be sent,
// full_message will contain 'Test full message', all other stuff will be lost
Yii::info([
    'test1' => 123,
    'test2' => 456,
    'short' => 'Test short message',
    'full' => 'Test full message',
    'additional' => [
        'additional1' => 'abc',
        'additional2' => 'def',
    ],
]);
```

- 方式二：使用 `\yiier\graylog\Log`

```php
<?php
\yiier\graylog\Log::info(
    'Test short message',
    'Test full message'
);


\yiier\graylog\Log::info(
    'Test short message',
    'Test full message', 
    [
        'additional1' => 'abc',
        'additional2' => 'def',
    ],
    'graylog'
);
```

详情参考 [yiier/yii2-graylog-target](https://github.com/yiier/yii2-graylog-target)

## 与 EFK 对比

没有深度研究，不知道两种方法哪个好？我所知道的：

- Graylog 作为专门的 log 收集工具，自带日志报警功能（具体使用方法，以后再补充），而据我所知 EFK 自带是没有这个功能的，为此我们项目组同事还特意写了一个小项目实现这个日志报警功能。
- 我们公司为了节省服务器开支把 EFK 换成 Graylog 了。

## 最后

最后说明一下为什么又重现造了一个 Yii 扩展的轮子？

因为刚开始用 [RomeroMsk/yii2-graylog2](https://github.com/RomeroMsk/yii2-graylog2)，照着文档使用，使用命令行调试，发现死活写不成功日志。而且这东西调试起来非常麻烦，因为这个库代码异常了，什么错误信息都看不到。

最后才发现问题出现在文档里面 `return $yii->request->getUserIP();`，这段代码不支持命令行方式运行，更加严谨的方式是 `return ($yii instanceof \yii\console\Application) ? '' : $yii->request->userIP;`。

当然上面那个结论我是最后才发现的，在这之前我不得已去找了其他库，找了半天，找到了 [alexeevdv/yii2-graylog-target](https://github.com/alexeevdv/yii2-graylog-target)，然后发现这个库，文档安装方式写错了，这不是问题，我还热心的给他提了 [PR](https://github.com/alexeevdv/yii2-graylog-target/pull/1)，因为他代码写的好，单元测试满分。但是这个库有两个坑
：
- 代码写的太『优雅』了，以至于按照文档方式使用 PhpStorm 却是提示代码异常。
- 另外一个问题就是他不支持扩展标签，比分说 `tag`, 多项目使用一个日志收集肯定要用 `tag` 区分的。

索性我就在他们两个的基础上重现造了轮子，并且增加了 `\yiier\graylog\Log` 使用方式。