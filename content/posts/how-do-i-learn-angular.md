---
title: "我是如何学习 Angular"
date: 2017-08-03T15:59:00+08:00
tags: ["经验分享", "Angular"] 
draft: false
toc: true
---

## 交代我的前端背景

工作5年多了，基本上都在写 PHP，对于前端在校的时候学过一些 JavaScript、HTML 和 CSS，那个时候我 HTML 和 CSS 还是比较擅长的。

后来第一家公司是外包公司，页面都是我们写的。那个时候 jQuery 还是非常火的，但我傻傻分不清楚 JavaScript 和 jQuery 以及 Ajax 他们之间的区别。而我们的前端代码全靠复制百度搜索出来的结果，后来公司终于招来了一位前端工程师，可算是把我们从不明所以的前端坑里面解救出来了。

后来我就很少接触前端了，但是不是完全不接触，花了10块钱买了本二手的《锋利的 jQuery》，才知道 jQuery 还是很好上手的。再后来业余有时间我会看一下 ES6、React 和 Angular 的文档，然后跟着写写。
但是因为主要工作还是 PHP，再加上一个人的精力有限，所以大部分时间还是在写 PHP，前端一直出于关注状态。

CSS 基本被我忘记的差不多了，好几年不写了，就越怕写 CSS 了。

<!--more-->

## 学习的步骤

用了一周时间使用 Angular 写了一个小工具网站：[iDevTools](http://idevtools.org/random?platform=blog) ，下面我就分享一下我是如何学习的。

- 先学 ES6，我是看这个教程的 - [JavaScript教程](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)，也没有完全看完，只看了前面一半。
- 在图书馆借到了这本书 [《揭秘Angular 2》](https://book.douban.com/subject/26945538/)。
- 看视频教程。主要是 [Learn Angular 4 from Scratch](https://coursetro.com/courses/12/Learn-Angular-4-from-Scratch) 和 [Angular 4](https://codecraft.tv/courses/angular/)，视频做的非常棒。听不懂他们说的英文不要紧，看操作步骤就可以了。
- 边学边写代码，你可以先给自己找一个小需求去实现，遇到问题就 Google，边写边学。
- 做学习笔记，把学习 Angular 的经历记录下来，我使用的是印象笔记。
- 刚开始一些概念不懂不要紧，先把功能写出来，有了成就感再慢慢优化代码。

## 为什么是 Angular？

React 我也尝试着去学了，但是感觉还是没上手，而且很不喜欢 JS 代码里面嵌套 HTML。 不学 Vue 的原因是怕以后用 Google 搜索不到自己的问题。

可能是因为跟我从事后端开发的原因有关系吧，刚学 Angular 时候就上手了。`ng g` 命令生成代码功能，深得我心。

在这里再顺便吐槽一下 Angular 的优缺点吧。

**优点**

- 大而全，而不是跟 React 一样只是一个视图层，需要路由功能还需要自己再手动添加。
- Angular cli 非常给力，让上手门槛很低。
- TypeScript 语法，我基本上没怎么专门的去看 TypeScript，都是按照 ES6 来写代码的。
- 模块化开发，让代码可用性高。指令功能很实用。

**缺点**

- 由于 Angular2 出来太慢了，导致很多前端工程师都去玩 React 和 Vue 了，然后相对来说 Angular 社区的周边开源就少了。我们刚开始的时候到处找各种组建，后来才发现 [PrimeNG](https://www.primefaces.org/primeng/)，跟捡到宝一样。
- 对于只会 jQuery 的前端工程师来说，上手门槛比较高，他们还停留在操作 DOM 的思想上。学习 Angular 要把之前的思想抛弃掉。

## 总结

我是一个写了5年多了后端工程师，花了一周时间边学边写，写了 [iDevTools](http://idevtools.org/random?platform=blog)。

学习步骤是，先学 ES6，在看书，然后去 YouTube 上搜索学习教程，跟着写。边学边写边记录笔记。