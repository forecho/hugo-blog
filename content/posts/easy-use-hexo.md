---
title: "手把手教你不依赖任何环境使用 Hexo"
date: 2016-10-27T20:58:00+08:00
tags: ["经验分享"] 
draft: false
toc: true
---


## 引言

今天心血来潮，想搭建一个 [3li3](http://3li3.com/) 的博客，准备以博客的形式记录产品的成长。类似的博客有：[SelfStore 博客](http://blog.selfstore.io/)。

感觉 Hexo 的博客插件比较多，于是这次想试试。因为很早以前就搭建过一次，这次以为分分钟搞定，但实际上还是花了我不少时间，为了以后不再重复踩坑，我决定记录下整个过程。

我们先看一下成果[ 3li3博客](http://blog.3li3.com/)，然后我再来分享我的经验。

<!--more-->

## 安装 Hexo

**方式一**

根据 [Hexo 官网](https://hexo.io/zh-cn/docs/)文档安装说明，你需要先安装 node 环境。
由于我不是专业的前端，本地发环境没有 node，还好 hexo 文档有提供安装方法，而且[安装方法](https://hexo.io/zh-cn/docs/#安装-Node-js)非常简单：

```
wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
nvm install stable
```

然后[安装 Hexo](https://hexo.io/zh-cn/docs/#安装-Hexo):

```
$ npm install -g hexo-cli
```

但是实际上我安装的时候，`nvm install stable` 命令卡半天也没反应，进度很慢。而且第一次我明明安装成功了，但是输入 npm 的时候任然提示：

`The program 'npm' is currently not installed. To run 'npm' please ask your administrator to install the package 'npm'`

输入 hexo 的时候也是遇到：`hexo: command not found`

我一脸懵逼，只能重新再安装一遍。每次看到一推输出 `npm WARN ...` 我也很慌，它是安装成功了吗？

总结一下，就是搭建环境是巨耗费时间的，特别是你对它不是十分了解的时候，如果你敢时间的话，我们来换一种姿势安装看看。

**方式二**


直接下载我刚刚安装好的 [Hexo](https://github.com/WJTeam/hexo/archive/v0.1.zip) 包，解压，然后我们就可以继续下一个步骤了。

这个方式不需要你安装 node 环境，任何一台新电脑都能直接使用。当然也有一个缺点就是：本地不能预览，但是这功能我们需要吗？

配合 [Travis CI](https://travis-ci.org/) 实现自动发布，还要啥本地预览？？

## 使用 Travis CI 工具自动部署 Hexo

我安装完 Hexo 第二件事就是完成此功能，而且今天在这个功能上「浪费」了最多时间。

其实 Google 搜索[「配置travis来自动发布hexo」](https://www.google.com.hk/search?q=%E9%85%8D%E7%BD%AEtravis%E6%9D%A5%E8%87%AA%E5%8A%A8%E5%8F%91%E5%B8%83hexo&newwindow=1&safe=strict&biw=1920&bih=974&ei=yZYRWPazAoKR0gLkj6j4Bw&start=0&sa=N) 结果一大把，但是我看了几篇都没找到想要的，我要的很简单 - 用最少的步骤完成此功能。

下面我来简单分享一下我的步骤：


1、去 Github 提供的 [Personal Access Token](https://github.com/settings/tokens) 创建一个 Token，然后复制 Token 值，记录下来，后面会用到。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424161626.png)

2、在项目根目录下新建 `.travis.yml` 文件，代码如下：

```
language: node_js
node_js: stable

# Travis-CI Caching
cache:
  directories:
    - node_modules

install:
  - npm install  #安装hexo及插件

script:
  - hexo cl  #清除
  - hexo g  #生成

after_script:
  - cd public
  - git init
  - git config user.name "forecho"
  - git config user.email "caizhenghai@gmail.com"
  - git add .
  - git commit -m "Auto deploy from Travis-CI."
  - git push --force --quiet "https://${GH_TOKEN}@github.com/${GH_REPO}.git" master:gh-pages

branches:
  only:
    - master

env:
  global:
    - GH_REPO: 3li3/blog
```

以上文件你需要修改4处：

- `git config user.name`
- `git config user.email" `
- `GH_REPO`

3、然后就去 GitHub 新建仓库、push 代码，操作 git 命令参考如下：


```
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/3li3/blog.git
git push -u origin master
```

4、然后你要去 [Travis CI](https://travis-ci.org/account/repositories)，手动开启此项目的监控（没有看到的话，点左边的  Sync account 按钮）。

开启之后点击设置添加环境变量值，在 `Environment Variables` 处添加 `GH_TOKEN` 值为上面第一步生成的 token 值。

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424161645.png)

至此配置完成，以后你写文章就是要去 `source/_posts/` 目录下，参照已有文件规则手动新建一个文件，就可以开始写博客了，写完再次 `git push` 就可以实现自动部署了。


## 添加域名解析

这个很简单，首先你要在项目 **`source` 目录**下创建一个 `CNAME` 文件，写入你要解析的域名就可以了，比方说：

```
blog.3li3.com
```

然后你需要在域名管理中心，添加一个 cname 解析，记录值写 `3li3.github.io`，3li3 代表 GitHub 组织名或者用户名，如下

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20190424161658.png)

## 更换主题

这个也遇到一个坑，首先我看到这篇文章 [Hexo Top 10 Popular Themes](https://en.abnerchou.me/Blog/5c00ca67/)，然后找到了 [NexT](http://theme-next.iissnan.com/) 主题，下载主题的时候一定要使用「下载稳定版本」的方式，不然你会遇到 git 子模块的坑。

然后使用上面的配置会导致 Travis CI 不能自动部署 Hexo，除非你让配置支持子模块，哎，懒得折腾了，所以我用最简单的方式把主题重新安装整了一遍。

关于主题的时候，文档已经很明确了，我就不多讲了。

## 总结

为了以后方便，一定要**使用 Travis CI 工具自动部署 Hexo**。 最后大家可以去参考 [3li3/blog](https://github.com/3li3/blog) 项目的源码。