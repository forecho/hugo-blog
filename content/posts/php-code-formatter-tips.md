---
title: "PHP 代码规范之工具篇"
date: 2018-10-12T20:50:00+08:00
tags: ["phpstorm"] 
draft: false
toc: true
---

## 前言

团队合作开发，保证代码的规范、统一是一个非常重要的事情。除了有文档明确的说明规范之外，我们还可以利用一些工具来辅助我们轻松实现代码的规范和统一。

<!--more-->

## Phpstorm


### 设置 PSR2 代码风格

Phpstorm 依次点击菜单栏 `File -> Settings -> Editor -> Code Style -> PHP` 找到 `Set from` 选择 `Predefinded Style -> PSR1/PSR2`

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424163723.png)


只有当使用 `Ctrl + Alt + L` 才会按照上面代码风格自动格式化。

### 录制宏

如果没有使用 `Ctrl + Alt + L` 的习惯的话，你可以使用录制宏的方式，制定『当我们按 `Ctrl + S` 保存的时候自动执行代码格式化』的规则：


- 点击 `Edit -> Macros -> Start Macro Recording` 开始录制宏，然后依次按快捷键 `Ctrl + Alt + L`、`Ctrl + Alt + O` 、`Ctrl + S` (Mac 对应的是 option + command + L、option + control + O 、command + S)。
- 然后点击 `Edit -> Macros -> Stop Macro Recording` 结束录制。会自动弹出一个窗口，自己设置个名称， 比方说 `Super Save`。
- 然后点击 `File -> Settings -> Keymap` 修改快捷键，把之前保存快捷键修改为 `Alt + S` 或者其他快捷键或者取消。然后搜索 `Super Save` 添加快捷键为 `Ctrl + S` 就可以了。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424162037.png)

## [GrumPHP](https://github.com/phpro/grumphp) + [PHP CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)

### 安装

```sh
composer require squizlabs/php_codesniffer --dev
composer require phpro/grumphp --dev
```

- [PHP CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) 是一个代码风格检测工具，可以实现检查代码是否符合 PSR2 规范。
- [GrumPHP](https://github.com/phpro/grumphp) 配合 Git 强制规范代码格式，提供 Git Hooks 来检测每次提交的代码，格式不通过 Git Commit 执行失败。


### 使用

Composer 执行完毕之后，会自动在项目根目录生成 `grumphp.yml` 文件，而且会自动添加 Git Hooks，具体可以查看项目根目录的 `.git/hooks/pre-commit` 和 `.git/hooks/commit-msg` 文件代码。要想到达我们想要的效果就得改 `grumphp.yml` 文件，修改之后的代码：

```
parameters:
    git_dir: .
    bin_dir: ./vendor/bin
    tasks:
      phpcs:
        standard: PSR2
        ignore_patterns:
          - ./resources/*
          - ./database/*,
          - ./bootstrap/*,
          - _ide_helper*
          - ./vendor/*
```

- `standard: PSR2` 使用 PSR2 标准。
- `ignore_patterns` 是不检测的目录或者文件。


安装完之后，我们还可以直接在项目根目录使用 phpcbf 的命令帮我们修复代码的规范：

```sh
./vendor/bin/phpcbf --standard=psr2 文件目录
```

## 扩展连接

- [PhpStorm Tips](https://phpstorm.tips/)
- [PHP版的代码整洁之道 中文翻译](https://github.com/php-cpm/clean-code-php)