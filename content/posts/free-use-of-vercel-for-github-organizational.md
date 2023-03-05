---
title: "GitHub 组织项目免费使用 Vercel"
date: 2021-10-18T18:09:00+08:00
tags: ["经验分享"] 
draft: false
toc: true
---

## 引言

[Vercel](https://vercel.com/) 是一个静态网站托管项目，而且有很多东西自动化完成，反正就是非常的方便和省事，我的[港美股平台推荐](https://stock.forecho.com/)网页就是使用 Vercel 托管的，访问速度很快吧。

Vercel 托管 GitHub 个人项目是免费的，不管私有还是开源，这还是比较良心的，但是组织项目是收费的，需要在 Vercel 创建 Team 才能使用，但是吧价格太贵了，对于目前没有收入的我来说有点承受不起，于是就找到了一种方式可以免费托管 GitHub 组织项目代码的方式。

<!--more-->

## 操作步骤

### 本地环境搭建

本地先有 [Nodejs](https://nodejs.org/) 环境，然后执行下面代码安装 Vercel

```
npm i -g vercel
```

### 搭建项目

然后就开始本地搭建项目了，不管你是博客还是文档都可以，支持的 [template](https://vercel.com/new/templates) 有很多。

比方说我想搭建一个文档，我就找到了官方的[文档示例项目代码](https://github.com/shuding/nextra)，下载了一份直接就可以用了。

### 创建 Velcel 项目

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211018e1a6SM.png)

在网页上我们只能通过导入 GitHub 项目的方式创建 Velcel 项目，免费版不能在线使用 GitHub 组织的代码，所以我们要使用命令行创建。

在上一步准备好的项目根目录执行 `vercel` 命令，然后按照提示一路操作就可以了，选项我都是选默认的，这个要按照项目情况选。

然后使用 `cat` 命令获取 `orgId` 和 `projectId` 的值

```
cat .vercel/project.json
```

### 添加 GitHub Actions

在当前项目添加 Github Action 部署文件 `.github/workflows/deploy.yml`，实现自动化部署，代码如下：

```
name: deploy website preview
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: amondnet/vercel-action@v20
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-args: '--prod' #Optional
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID_STATIC }}
      - name: preview-url
        run: |
          echo ${{ steps.vercel-action.outputs.preview-url }}
```

根据你的需求需要适当的修改。

### 添加 GitHub Actions secrets

![](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/20211018Xyc06u.png)

给 GitHub Actions 添加环境变量，在 GitHub 项目的设置里面找到相应的位置创建，我们只需要创建三个值：

- `VERCEL_ORG_ID`：前面获取到的 `orgId`
- `VERCEL_PROJECT_ID_STATIC`：前面获取到的 `projectId`
- `VERCEL_TOKEN`：在 Velcel 手动[创建 token](https://vercel.com/account/tokens)

然后每次 `push` 代码，Velcel 就会自动部署了。


## 最后

这几步虽然操作起来不算麻烦，但是也费时间，有能力的朋友尽量还是购买 [Vercel Pro](https://vercel.com/pricing) 吧。

最后感谢 [Vercel](https://vercel.com/) 和 [amondnet/vercel-action](https://github.com/amondnet/vercel-action) 提供的服务。