---
title: "使用 Linear 来管理你的项目"
date: 2021-10-20T10:59:00+08:00
tags: ["经验分享", "项目管理"]
draft: false
toc: true
---

[![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211020VXxM9t.png)](https://linear.app/)

## 引言

互联网时代，大家做软件开发通常都是使用[敏捷开发](https://zh.wikipedia.org/zh/Scrum)的模式，那么告别小作坊式的软件开发应该从使用项目管理工具开始。

从事 WEB 开发接近 9 年了，这些年来从刚开始的公司没有项目管理工具，到后来用过 [Trello](https://trello.com/)、[TAPD](https://www.tapd.cn/) 、[Teambition](https://www.teambition.com/) 、[Github Projects](https://github.com/cashwarden/web/projects) 、[Jira](https://www.atlassian.com/software/jira) 等不计其数的产品。

<!--more-->

今天我正式给大家推荐到目前为止我用过的最满意的项目管理工具  - [Linear](https://linear.app/)，下面我就挑几个本人喜欢的亮点谈谈：

## 多种视角

可以从多种视角获取项目的进度：

### 清单模式

可以让你专注于当下任务。

### 看板模式

可以让你一览项目整体情况。

### 甘特图模式：

了解项目的整体进度，防止延期。

## 自动化

这里指的是配合 GitHub、GitLab 实现任务状态自动更新的功能，使用此功能要先配置 GitHub、GitLab，其他配置可以使用默认的也可以自定义。

此功能是我最喜欢的功能了，而且这也是我使用 Linear 最大的原因。这是一种工作流程，大概流程就是：

- 先在项目管理里面创建一个 Issue，也就是任务
- 然后代码里面，新建一个分支开发任务，关键在于这个分支的命名，必须要按照约定的规则命名。
- 开发完成之后提交 PR (Pull request) 然后状态就会自动更新为「In Progress」
- PR 被合并之后，任务的状态就自动更新为「Done」
- 最重要的是 PR 页面会自动贴上 Linear 任务单连接，Linear 任务单这边也会自动补上 PR 单，非常方便回顾和查看

详情介绍可以去看官网文档 - [《GitHub and GitLab》](https://linearapp.notion.site/GitHub-and-GitLab-fa4b88df484343e4989538f066c729f3)

PS：上家公司用的是 Jira 管理项目，当时就体验过，用过这个功能之后就觉得「真是太棒了」，但是 Jira 没有提供免费版本，而 Linear 可以免费使用。

## 开放生态

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211020XuH2yo.png)

国内的话生态太差了，很多软件都不会打通，Linear 目前支持以上几个平台。

举个例子 Linear 连接 [Slack](https://slack.com/intl/zh-cn/) 之后，如果有任务更新状态会自动给 Slack 发消息，方便跟团队其他人同步消息。

像上面说的自动化功能也是得益于生态的开放，反正我没看到有一家国内项目管理有此功能的。

## 对小团队更友好

Jira 功能强大，也很复杂，没有免费版，不太适合小团队。而 Linear 对小团队非常友好，免费用户功能很良心。具体区别你可以看看这个页面 - [Pricing](https://linear.app/pricing)

## 最后

知道这个工具还是得益于推友 [@61](https://twitter.com/liuyi0922) 的推文，然后去体验了一把，确实很给力，而且还可以免费使用我一直在寻找的自动化功能，真的非常感谢。

另外想补充的有：

-  GitHub Projects 其实也有自动化功能，但是操作有点慢（网络问题？），其他功能（比方说统计，甘特图）也没有。
- 还有一款类似的项目管理工具 - [Height](https://height.app/) ，大体功能差距不大，但是设计理念有点区别，有兴趣也可以去体验一下。