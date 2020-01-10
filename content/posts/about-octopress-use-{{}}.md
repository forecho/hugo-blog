---
title: "Octopress 显示 `{{}}` "
date: 2019-03-06T21:58:00+08:00
tags: ["Octopress"] 
draft: false
toc: true
---

**注意：** 本篇文章需要点击[这里](https://github.com/forecho/blog/blob/master/source/_posts/2019-03-06-about-octopress-use-%7B%7B%7D%7D.md)才能正常阅读。

Octopress 使用双花括号的时候就会自动把他认为是变量，会『翻译』，但是其实我们并不想要这种效果，怎么办呢？网上找了一圈发现可以用下面这种方式解决

```
{%raw%}{{thing}}{%endraw%}
```

<!--more-->

但是这样的话对去 [Github 对应文章](https://github.com/forecho/blog/blob/master/source/_posts/2019-03-01-use-postman.md) 阅读的体验造成很大的破话。解决办法就是把整个文章都包含进去即可：

```

{%raw%}

文章内容 {{thing}} 文章内容

{%endraw%}

```