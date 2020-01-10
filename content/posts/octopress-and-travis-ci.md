---
title: "Octopress 结合 github 和 travis-ci 实现自动更新博客"
date: 2015-07-21T17:25:00+08:00
tags: ["octopress", "travis-ci"] 
draft: false
toc: true
---
## 前言

这么久没更新博客了，一是因为 Octopress 虽然在自己的  MacBook 搭建起来了，但是工作电脑还没有，最主要的是工作电脑是 Windows 原来安装 Octopress 本身就是一件蛋疼的事情。对于一个懒人的我来说，一直想找一个自动生成静态博客并且会自动发布的解决方案，差点都准备放弃 Octopress 了，直到今天上午这种方案被我找到，本来以为最多一上午能搞定，没想到花费了我一天时间。


## 实现

主要是利用 travis-ci 提供的持续集成服务实现的，主要分三步就搞定：

1. 用 github 登录[Travis-CI](https://travis-ci.org/),然后开启你要使用集成服务的项目。
2. 添加 `.travis.yml` 文件（octopress 默认就有这个文件，我们修改就可以了）。
3. 只要配置通过，当你 `git push` 的时候就会自动触发 travis-ci，后面的工作就全自动了。

<!--more-->

### .travis.yml

详细讲一下这个文件，我全部的时间都在测试这个。先看最终版本代码：

```
language: ruby
rvm:
  - 2.0.0
before_script:
  - git config --global user.name "forecho"
  - git config --global user.email "caizhenghai@gmail.com"
  - export REPO_URL="https://$GH_TOKEN@github.com/$GH_REPO.git"
  - bundle exec rake setup_github_pages[$REPO_URL]
  - git checkout -- _config.yml
script:
  - bundle exec rake generate
after_script:
  - bundle exec rake deploy
env:
  global:
  - GH_REPO="forecho/blog"
  - secure: Ey/zcMQ6foHssfLpl1O0YATSYXRfDLZBGiYo4r+qlLsmMwokmTK9EyfkQA1Ycyxh3nuFA0Dtn68w+rRj5NXNDK5xd0vkNWME/kHzMo/YpBlJXT0qXWZR9BTqLp88gLPTgYsAu+cBmSoxTbbpRIGL4GE4BRfdk9Tci0QnKfFtN/cS0BoVGI/NH4cCpgh7dBJ/f3k9sUR6mOWuese0BtkxsNoi3ViNFLB+ao4VuReIM6DzDSi5+1/WTiDK52t+0iVoyMorvSEDU3wpxAng7iABcti/CFFhAbg188tCvP+ZkEQUlSmZBQ4oSKNpobSLRQqmopnxZvdkagH/RPk0oth4epJmsIY+gre/HHAsj8bpAROJE/48GI3sY539FFNxy4LtrtWivfOcBF+alpAAWEER3Ktf0Qj7g0WuyGcqKaTeYApropmM5Fpukj1uibBaRyzNSihbY411f9lFzxnzHtNXjqJ0ZnZfZ70u6Oy3+IpJFxkRloiQMwWbg6mCOyKOXhhZ2mmpQu7oJFhLvb+i9QkmJ+v7LiwjBOpkPVRs40jbB078kKE17mOJyBgwKxjYqIY6ycYoJGUr+cQKynQTx67+Md35EC9lqm25a2wjHVlVpVlZ3oUO8q3EGByB1ShtLOQxhfg+a2seH4PK0NkCq7pnHPEiOZfAGIjJ+ayxPWpnDPA=
```

**说明**：

- ruby 要用 2.0 版本，不要用 1.9.3 版本，不然你会遇到 `incompatible encoding regexp match (ASCII-8BIT regexp with UTF-8 string)` 编码问题，就这个问题我浪费了好多时间。
- secure 是要自己生成的，生成教程参考这个 [打造Octopress博客在线写作平台](http://xuhehuan.com/1761.html)。

## 其他问题

Q: GitHub Pages 可以配置当独的域名吗？


A: 当然可以，多个 GitHub Pages 项目可以每一个配置独立的域名，只要你能保证 GitHub Pages 项目根目录下有一个 `CNAME` 文件，里面写上域名就可以了。Octopress 的话，只要保证 `source` 目录下有 `CNAME` 文件就可以了。


## 总结

如果你不想依赖任何 VPS 和 虚拟空间使用一个博客的话，可以试一下我这个方法。唯一花钱的就是买一个域名。


**参考文章:**

1. [用Octopress在GitHub上搭建博客](http://wangmuy.github.io/blog/2013/09-01-octopress-setup.html)
2. [打造Octopress博客在线写作平台](http://xuhehuan.com/1761.html)
3. [How to Start Blogging Easily With Octopress and Teracy-dev](http://blog.teracy.com/2013/08/03/how-to-start-blogging-easily-with-octopress-and-teracy-dev/)
4. [teracyhq/blog 项目](https://github.com/teracyhq/blog)

