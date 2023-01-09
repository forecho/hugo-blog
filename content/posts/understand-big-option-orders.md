---
title: "读懂期权大单"
date: 2022-12-22T12:35:00+08:00
tags: ["深度理财", "期权"]
draft: false
toc: true
---

## 引言

玩美股期权差不多有一年了，今天给大家分享一下，我是在哪里看到的期权大单，以及如何读懂期权大单。

本篇文章适合有一定期权基础的读者。如果你还不懂什么是期权？可以去看看我的[期权系列](https://blog.forecho.com/tags/%E6%9C%9F%E6%9D%83.html)文章。

## 在哪里看到期权大单

### [OptionStrat](https://optionstrat.com/flow/live?ref=caizhenghai)（力推）

![](https://img.forecho.com/HrmDqQ.png)

OptionStrat 是期权交易者不错的工具，使用 OptionStrat 直观的可视化和分析工具进行更智能的交易，免费版用户可以查看延迟 15 分钟的期权流。

### [barchart](https://www.barchart.com/options/unusual-activity)

<!--more-->

![](https://img.forecho.com/IAKLxB.png)

BarChart 有免费提供一个异常期权活动的页面，可以看到当天的期权大单。

> 异常期权活动是指相对于合约的未平仓合约而言，交易量较大的期权合约。不寻常的期权活动可以提供对「聪明的钱」正在做的大批量订单的洞察力，预示着新的头寸和潜在的相关资产的大动作。当最后的价格在卖出价或高于卖出价时，执行价格以绿色显示；当最后的价格在买入价或低于买入价时，执行价格以红色显示。

### [unusualwhales](https://unusualwhales.com/)

![](https://img.forecho.com/0LCp2z.png)

UnusualWhales 是一个收费的期权大单监控平台，可以看到当天的期权大单。免费版可以看到最近 7 天的期权大单。

付费版功能强大，可以看实时的期权大单，还可以看到期权大单的历史数据。而且还可以看到期权大单的分布，可以看到期权大单的分布在哪些股票上，哪些股票的期权大单比较多。

可以买终身会员，不过最近涨价了，现在终身会员要 3000 美金。

### 证券 App

![](https://img.forecho.com/m3trQx.PNG)

据我所知，富途牛牛和华盛证券都有这个功能，数据可能不全，但是可以看到当天的期权大单。作为一个免费的功能，还是很不错的。

当然使用这个功能的前提是你有在使用他们的证券 App，不然要付费使用。

### Twitter

![](https://img.forecho.com/VE4bj8.png)

Twitter 上有很多分享期权大单的账号，比如：

- [Cheddar Flow](https://twitter.com/CheddarFlow)
- [Peter Tarr](https://twitter.com/ProfitsTaken)
- [Option Waves](https://twitter.com/optionwaves)
- [Chameleon](https://twitter.com/MarketChmln)
- [unusual_whales](https://twitter.com/unusual_whales)
- ……

我在 Twitter 上新建了一个[期权列表](https://twitter.com/i/lists/1606299260877955072)，里面有很多期权大单的账号，有兴趣的朋友，可以关注一下。另外也欢迎大家推荐一些期权大单的账号。

## 如何读懂期权大单


### 期权大单的交易类型

![](https://img.forecho.com/o3l0LP.png)

期权大单的交易类型主要有以下三种：

- Block Order: 大宗交易
- Split Order: 单交易所分割交易
- Sweep Order: 扫单交易

**Block Order**

Block Order 是指一次性交易大量的期权合约，比如一次性买入 1000 个合约，这种交易一般是由机构投资者进行的。在非公开市场进行，且私下交易，交易完之后才去交易所登记清算。

好处是可以用单一的价格买入大量的期权合约，避免了市场上的价格波动。

**Split Order**

Split Order 是指机构或者大户在一家交易所上短时间内分批次的买入或者卖出期权合约，然后变成一个大单交易记录。

因为这种交易很明显就能识别交易者的情绪。

**Sweep Order**

Sweep Order 是 Split Order 的升级版本，是指机构或者大户在多家交易所上短时间内分批次的买入或者卖出期权合约，是短线交易者的常用手段，因为它可以隐藏交易者的情绪。

### 期权大单的交易方向

![](https://img.forecho.com/KhqXFB.png)

怎么看期权是买入还是卖出？unusualwhales 有显示 Side（有的平台没有），对于 Call 来说：

- BID: 买入
- ASK: 卖出
- MID: 买卖均价

对于 Put 来说：

- ASK: 买入
- BID: 卖出
- MID: 买卖均价

除了显示 Side 之外，unusualwhales 还有一个 Emojis 栏，可以很直观的看出是看涨或者看跌亦或者是中性。

当然除了 unusualwhales 之外，富途牛牛、华盛证券和 OptionStrat 等也有显示交易方向（是看涨还是看跌），华盛证券和 OptionStrat 甚至会显示是组合期权。

## 期权大单的交易策略

期权大单说明了市场的情绪，但是并不能说明市场的走势，所以我们需要结合其他的指标来判断市场的走势。大量的买入 PUT，说明市场情绪是看跌，但是并不能说明市场会跌，也有可能是用来做做空的保险。

但有时候确实能从期权大单的不寻常的交易中看出一点点猫腻，总有人会提前知道一点消息，然后埋伏在期权市场里，等着消息公布之后，股票就会巨大的波动，然后获得巨大的利润。

## 总结

期权大单是期权市场的一大特色，它能够很直观的反映市场的情绪，但是并不能说明市场的走势，所以我们需要结合其他的指标来判断市场的走势。

最重要的是期权的大单交易流只是一个帮我们做交易决策的工具，最终的交易决策还是要靠自己的判断，所以不要随意的跟单。