---
title: "参加 OpenResty 技术沙龙活动"
date: 2019-08-17T21:48:00+08:00
tags: ["分享"] 
draft: false
toc: true
---

![](https://i.loli.net/2019/08/19/MnqFwm5hd4GQEAK.jpg)

## 引言

起因是公司另外一个项目组老大说，章亦春（下文简称春哥）最近回国，他可以请春哥来公司做个小分享会。之前就对春哥略有耳闻，这次可以见他本人，也是难得的机会。

前两天听完春哥在公司的分享，两个小时他讲了很多他的编程经验和经历，听完收益很大，但是两个小时根本不够他讲，我也没听过瘾，于是又报名了他的[《OpenResty 2019 深圳技术沙龙》](https://www.huodongxing.com/event/7502793207200)活动，又跑去听了他讲了 5 个小时。

这篇文章我主要是分享一下他分享的几个点（仅限于我能理解和认为的），以及我的收获。

> 如果你还不知道春哥是谁的话，可以先读读这篇文章[《从抄书到开源之巅：章亦春的程序人生》](http://www.ituring.com.cn/article/504549)

<!--more-->

## 关键词

### Perl

春哥很早就接触了编程，说是1999年。而且他非常喜欢 [Perl](https://www.perl.org/) 语言。到现在他也一直在用 Perl，甚至自己去实现了 Perl6 的方言，他称之为 [Fanlang](https://doc.openresty.com.cn/en/fanlang/)，效率比 Perl6 要高，不开源。

这之前我一直以为 Perl 是一个过时的语言，没想到现在还有人用。这次听完分享会，也看了春哥现场写的代码，改变了我对 Perl 的印象，决定去学一学。

### 阅读

春哥抄书学习，大家都有听过了。抄书、抄代码学习的关键在于**延缓阅读速度，不至遗漏每一个重要的细节：眼到，手到，心到。**

至于阅读，春哥有自己的一套方法，先阅读 30% 的内容，再猜测结论，然后接着阅读剩下的 70%。有时候猜测的结论与作者截然相反，即撞车的情况，但有时候会出现与自己的猜测一直的情况。

再有就是多读书，多读一手的书（即英文原版），远离二手知识，会影响独立思考。

### 编程

多学习和接触不同的语言，每一类语言都有自己的思维方式，除了可以开阔眼界，也可以开阔思维。春哥是推荐一个月学习一门语言。这里我推荐[《七周七语言：理解多种编程范型》](http://www.ituring.com.cn/book/829) 这本书。

上面做的一类语言，可以理解为 PHP、JavaScript、Perl 等脚本语言算一类语言，[Haskell](https://www.haskell.org/) 等函数式又算另一类语言。

春哥自己发明了很多小语种，每一种小语种做一个领域的事情。想在线体验这些语言可以访问 <https://demo.openresty.com.cn/langs/fanlang> 体验，还有文档，弹出要输入 Email 的输入框，随便输入一个以 `@openresty.com` 结尾的邮箱即可。这些小语言的编译器都是春哥用上面提到的 Fanlang 实现的。

特别是给我们大家演示 [Navlang](https://doc.openresty.com.cn/en/navlang/) 的时候，我被惊艳到了。Navlang 可以实现基于视觉的自动化操作，可以用来自动化测试、也可以用来写爬虫、还能用来写产品文档，这个时候还可以自动录音、截图。你只要负责写脚本，剩下的交给 Navlang 就可以了。[Mini CDN Demo](https://doc.openresty.com.cn/en/mini-cdn-demo/ep01-intro.html) 的文档就是用这种方式实现的，反正我对这个语言最感兴趣了。听说他们 9 月份上线（付费使用），到时候可以想试试。

能写编译器的程序员不简单的，必须要有很强的能力。如果对编译器感兴趣的朋友，春哥推荐三个：

- 一本书：[《Engineering a Compiler, Second Edition》](https://book.douban.com/subject/5288601/)
- 学好正则表达式
- 学好 Linux 的 awk 命令，特别是多看看复杂的 awk 命令

另外就是在学习软件开发的时候可以多看看源码或者参与开源项目，在参与开源项目之前可以先看看 [《The Practice of Programming》](https://book.douban.com/subject/1459281/)。代码都是有气味的，在参与开源项目之前先把自己的气味修正，即多注重代码细节。

### OpenResty

命令的由来，是因为当初 OpenAPI 这个词非常火，然后 `resty` 是来自于 `restful`。

第一代 OpenResty 是基于 Perl。第二代 OpenResty 是基于 Nginx + Lua。

现在几乎所有的 CND 服务都是基于 OpenResty，这得意与春哥在 [CloudFlare](https://www.cloudflare.com/) 工作了四年多的原因。


### 机器写代码

春哥高中的时候就开始开发程序让机器解高考题。后来自己创业开公司也是因为想继续做这方面的事情，让机器写代码。这个挺有意思的，包括他讲述他在淘宝根据文档生成代码的故事，让我大开眼界。


### 性能优化

技术沙龙的时候讲了很多这方面的点，由于我没有从事这方面的工作，就记录以下几个关键点：

- DTrace：动态追踪技术
- [systemtap](https://sourceware.org/systemtap/)：内核调试神器
- [Brendan Gregg](http://www.brendangregg.com/)：性能优化领域专家

### 其他

- [google/AFL](https://github.com/google/AFL): 模糊测试工具
- [Olivine-Labs/busted](https://github.com/Olivine-Labs/busted): Lua 单元测试
- [Graphviz](https://www.graphviz.org/)：一个开源的图可视化工具
- [openresty/programming-openresty](https://github.com/openresty/programming-openresty)：一本基于 OpenResty 编程的书

## 最后

春哥再次让我体会到了，牛逼的程序员可以顶十个甚至百个普通的程序员。虽然春哥分享的经历我们能借鉴的不多（写编译器这种事情不是每个程序员都能做的），但是我们可以：

- 尝试他的阅读方法
- 多接触不同类型的语言
- 看他分享的两本书
- ……



