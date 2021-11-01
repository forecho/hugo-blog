---
title: "软件开发的工作流"
date: 2021-10-31T21:03:00+08:00
tags: ["经验分享", "项目管理"] 
draft: false
toc: true
---

## 引言

今天我打算给大家分享一下我上家公司的软件开发工作流，工作也 9 年了，这是我迄今为止觉得最舒服的工作流。

## 工作流

我们使用了工具主要有：

- [GitHub](https://github.com)
- [Slack](https://slack.com/)
- [Jira](https://jira.com/)

<!--more-->

### [Slack](https://slack.com/)

用过钉钉、企业微信、体验过 Microsoft Teams、Discord 等产品，个人觉得最适合企业办公的工具是 Slack（~~这也是为什么它刚上市的时候我就买了他的股票，虽然后来被割了~~）。

个人认为 Slack 最大的亮点在于：

**丰富的应用市场**

可以安装各种 App ，丰富的应用市场，生态非常好，我来说几个使用场景：

- 开发完一个功能之后我们会通过 PR (Pull request) 合并代码，Slack 配置上 GitHub 应用，打通两边的用户，当 PR 需要人 review 的时候，对应的人会收到 Slack 通知，而不是我提交 PR 之后再给对应的人发消息，让他去 review，据我所知企业微信、钉钉是没办法做到这点的。不管是提醒 PR 还是 PR 被合并，对应的人都会自动收到消息，这体验非常棒。没有这个功能只能依靠 GitHub 的 Email 提醒？
- 各种消息提醒，比如：代码成功上了 prod，错误预警等等，不过这种企业微信、钉钉也可以实现。

**更加友好的讨论方式**

每条消息都可以单独的回复讨论，非常方便深入讨论问题（我真的爱死这个功能了），方便多个话题同时讨论（在不同的 Thread 回复消息，互不干扰），而且这种讨论方式能做到最小干扰他人，Thread 里面有信息更新，只有相关人员才（不是整个讨论组的人）会收到通知

飞书、企业微信、钉钉都把「消息可以标记表情」功能抄过来了，不知道为什么他们不抄袭这个 Thread 消息讨论的功能？


![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211026LQWoJ2.png)


### [GitHub](https://github.com)

相信大家现在都在用 Git 管理代码吧，不是用 GitHub 就是用 GitLab 了，GitLab 其实也可以，但是 GitHub 功能更多一点，生态也好一些。而且现在 GitHub 免费账户，都可以在组织里创建私有代码库了。

不过锁定指定分支，不能直接 push 只能通过 PR 提交代码，这个功能还是收费的。如果团队开发还是有必要使用这个功能的。

### [Jira](https://jira.com/)

超强大的项目管理软件，不过没有免费版，可以通过配置可以实现与 GitHub 打通，一个任务一个分支开发，开发完成提交 PR 之后会自动更新 Jira 任务单的状态，而且任务单也会有  GitHub 对应的 PR 链接，方便回顾。

目前我发现 [Linear](https://linear.app/) 可以做到这几本的功能，虽然 Linear 功能还比较，没有 Jira 的冲刺和给任务单分配 Points 等功能，单基本够用。下面是 Linear 和 GitHub 的两张效果图：


![Linear 任务单](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211101kVkxJs.png)

![Github PR](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202111012LCJoC.png)

据我所知，Tapd 和 teambition 是没有打通 GitHub 的，他们只提供一个非常简单的关联功能，就是写 commits 消息的时候附带他们的任务 ID，但是这会有一个问题，写 commits 的时候经常会忘记附带任务 ID，而按照 Git 分支名称做自动化要合理的多。

## 其他

顺便推荐一下其它我在用的工具：

### [Sentry](https://sentry.io/)

如果小公司小项目，没有专业的测试人员，可以试试接入 [Sentry](https://sentry.io/)，无入侵的接入，可以说基本上是 0 成本使用，有了它你可以很清楚的了解线上产品的异常情况，达到快速响应。

如果项目做大之后可以考虑接入 [EFK](https://www.elastic.co/products/enterprise-search)

### [Datadog](https://www.datadoghq.com/)

这是一个基于 SaaS 的数据分析平台提供对服务器，数据库，工具和服务的监视。在你的项目达到一定规模，可以考虑使用这个，能帮助你节省 DevOps 人力。

### CI/CD

持续集成和持续交付是现代化开发的必备了，推荐使用 [GitHub Actions](https://github.com/features/actions) 或者 [Travis CI](https://travis-ci.org/)，如果是 GitLab 
就是 Runner 了，当然也有自己搭建 [Jenkins](https://www.jenkins.io/)（不推荐）

我认为以下功能是最起码要有的：

- PR 自动跑单元测试
- 合并到指定分支（`master` 或者 `stage` 分支）实现自动化部署

## 总结

越是小的团队越是要自动化一切能自动化的。总结下来：

- Slack 是目前没有能代替的企业沟通工具
- 代码 GitHub 或者 GitLab 都可以，但是 GitHub 生态和功能更友好
- 项目管理工具推荐使用 [Jira](https://jira.com/) 和 [Linear](https://linear.app/) 