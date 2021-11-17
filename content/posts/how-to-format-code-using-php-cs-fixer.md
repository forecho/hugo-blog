---
title: "使用 PHP CS Fixer 规范代码"
date: 2021-11-17T13:12:00+08:00
tags: ["代码规范", "phpstorm"] 
draft: false
toc: true
---

## 引言

之前我有写过[《PHP 代码规范之工具篇》](https://blog.forecho.com/php-code-formatter-tips.html)，这次我们来看看如何使用 PHP CS Fixer 进行代码规范。这篇文章可以做为补充。

## 安装

### 安装 [PHP CS Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)

全局安装：

```shell
composer require --global firephp/php-cs-fixer
```

<!--more-->

或者单个项目安装：

```shell
composer require firephp/php-cs-fixer --dev
```

### 自定义规则

```shell
vim .php-cs-fixer.dist.php
```

我目前使用的是基于 PSR12 修改的规则，可以参考，也可以自己去自定义规则，代码如下：

```php
<?php

use PhpCsFixer\Config;
use PhpCsFixer\Finder;

$finder = Finder::create()
    ->in([
        __DIR__.'/app',
        __DIR__.'/config',
        __DIR__.'/database',
        __DIR__.'/resources',
        __DIR__.'/routes',
        __DIR__.'/tests',
    ])
    ->name('*.php')
    ->notName('*.blade.php')
    ->ignoreDotFiles(true)
    ->ignoreVCS(true);

return (new Config())
    ->setRules([
        '@PSR12' => true,
        'array_syntax' => ['syntax' => 'short'],
        'ordered_imports' => ['sort_algorithm' => 'alpha'],
        'no_unused_imports' => true,
        'trailing_comma_in_multiline' => true,
        'phpdoc_scalar' => true,
        'unary_operator_spaces' => true,
        'binary_operator_spaces' => true,
        'blank_line_before_statement' => [
            'statements' => ['break', 'continue', 'declare', 'return', 'throw', 'try'],
        ],
        'phpdoc_single_line_var_spacing' => true,
        'phpdoc_var_without_name' => true,
        'method_argument_space' => [
            'on_multiline' => 'ensure_fully_multiline',
            'keep_multiple_spaces_after_comma' => true,
        ]
    ])
    ->setFinder($finder);
```

PS：使用不同的框架，第一段代码会不一样，上面是针对 Laravel 的结构。

然后修改 `.gitignore` 文件，添加如下内容：

```
.php-cs-fixer.cache
```

### 安装 [GrumPHP](https://github.com/phpro/grumphp)

```shell
composer require --dev phpro/grumphp
```

`grumphp.yml` 配置如下：

```yaml
grumphp:
  tasks:
    phpcsfixer:
      config: .php-cs-fixer.dist.php
      verbose: true
      diff: true
      triggered_by:
        - php
```

### 添加 composer script

在 `composer.json` 的 `scripts` 中添加如下内容：

```json
"scripts": {
    "php-cs-fixer": "php-cs-fixer fix"
}
```

路径需要根据情况修改。

## 配置 Phpstorm

### 设置 `PSR12` 代码风格

Phpstorm 依次点击菜单栏 `PhpStorm Settings -> Editor -> Code Style -> PHP` 找到 `Set from` 选择 `Predefinded Style -> PSR12`

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211117SCXvnN.png)

### 添加 External Tools

这一步是方便自动化格式代码的重要步骤，我们可以在 Phpstorm 中添加一个 External Tools，这样我们就可以在 Phpstorm 中自动化格式代码了。

Phpstorm 依次点击菜单栏 `PhpStorm Settings -> Tools -> External Tools` 添加一个 External Tools，设置如下：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211117Z056E4.png)

```
name: "PHP CS Fixer"
description: "Apply php-cs-fixer to the current file"
program: "/Users/forecho/.composer/vendor/bin/php-cs-fixer" // 这里是你的 composer 安装路径
arguments: "fix $FileDir$/$FileName$" // 这里是你的 php-cs-fixer 命令
working_dir: "$ProjectFileDir$"
```

`Open console for tool output` 前面的 ✅ 去掉，不然会弹出一个输出结果的窗口，不方便。

### 使用 PHP CS Fixer 校验代码

Phpstorm 依次点击菜单栏 `PhpStorm Settings -> Editor -> Inspections` 选上 `PHP -> Quality Tools -> PHP CS Fixer validation`:

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211117J4SoI7.png)

图上`第 4 点`要自己选路径，我这里是 `/Users/forecho/Code/php/account/.php-cs-fixer.dist.php`，这里需要改成你自己项目的路径。

### 配置快捷键

Phpstorm 依次点击菜单栏 `PhpStorm Settings -> Keymap` 给 PHP CS Fixer 添加快捷键：

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211117HdJuXl.png)

### 录制宏

通过录制宏可以实现一个快捷键做几件事情，具体请看我之前的文章：[《PHP 代码规范之工具篇 - 录制宏》](https://blog.forecho.com/php-code-formatter-tips.html#%E5%BD%95%E5%88%B6%E5%AE%8F)

## GitHub Actions 自动修复代码格式

不是很推荐这种方式，但是如果有需要还是可以做到的。

在项目中添加文件 `.github/workflows/php-cs-fixer.yml`，内容如下：

```yaml
name: Check & fix styling

on: [ push ]

jobs:
  style:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v1

      - name: Fix style
        uses: docker://oskarstark/php-cs-fixer-ga
        with:
          args: --config=.php-cs-fixer.dist.php --allow-risky=yes

      - name: Extract branch name
        shell: bash
        run: echo "##[set-output name=branch;]$(echo ${GITHUB_REF#refs/heads/})"
        id: extract_branch

      - name: Commit changes
        uses: stefanzweifel/git-auto-commit-action@v2.3.0
        with:
          commit_message: Fix styling
          branch: ${{ steps.extract_branch.outputs.branch }}
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 最后

多人团队合作代码规范性非常有必要，代码风格的统一，越早执行越好。


## 参考资料

- [《КАК ОТФОРМАТИРОВАТЬ КОД В PHPSTORM ИСПОЛЬЗУЯ PHP CS FIXER》](https://si-dev.com/ru/blog/how-to-format-code-in-phpstorm-using-php-cs-fixer)
- [Phpstorm PHP CS Fixer](https://www.jetbrains.com/help/phpstorm/using-php-cs-fixer.html#installing-configuring-php-cs-fixer)
- [《Create a File Watcher for php-cs-fixer in PhpStorm》](https://eidson.info/post/phpstorm-file-watcher-php-cs-fixer)
