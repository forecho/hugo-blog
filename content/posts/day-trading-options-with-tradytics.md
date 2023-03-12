---
title: "使用 Tradytics 进行日内期权交易"
date: 2023-03-11T10:40:00+08:00
tags: ["深度理财", "美股", "期权", "tradytics"]
draft: false
toc: true
---

## 引言

前一段时间和几个朋友以一个非常低的价格拼车买了 [Tradytics](http://link.3li3.com/tradytics) 的会员，一有时间我就研究如何使用，虽然 [Tradytics](http://link.3li3.com/tradytics) 太复杂了，但是有很多有用的数据，可以帮我们看清目前的市场状况，然后做出正确的交易决策。

我会把我学到的东西分享出来，希望能帮助到大家。系列文章点击 [Tradytics](https://blog.forecho.com/tags/tradytics.html) 查看。

## Tradytics 是什么？

一个人工智能驱动的交易平台，提供了各种工具来帮助我们在市场上获得优势。

Tradytics 每天分析超过一百万个数据点，来自期权流量、股票日内数据、新闻、内部交易等。

基于这些分析，通过专有人工智能算法，提取有用信息，帮助我们提前评估市场走向。

<!--more-->

## 找到日内期权交易的标的

### 了解期权市场摘要

![](https://img.forecho.com/nDtDk2.png)

访问 [Options Market](https://tradytics.com/options-market) 了解市场的详细视图和统计数据，将有助于我们选择合适的交易方向。

上图是汇总的市场净流量是将市场中所有股票的净保证金与 SPY 股价进行比较，可用于预测市场方向。点是基于动量的进入点。请观看[视频指南](https://www.youtube.com/watch?v=HHkRGiqBKvg)了解更多详情。

我认为这是 Tradytics 最有用的功能之一，因为它可以帮助我们快速了解市场的走向。

### 顶级期权流量


![](https://img.forecho.com/FLTFbq.png)

从「TOP OPTIONS FLOW」开始：

1. 每日最佳期权流向：每日具有最高和最低权利金的股票。净权利金是看涨和看跌头寸之间的差额。

2. 每周热门期权流量：每周具有最高和最低净权利金的股票。净权利金是认购期权权利金与认沽期权权利金之差。



### 顶级期权溢价

![](https://img.forecho.com/JMBGZh.png)

1. 成交量变化最大的 CALL：相对于过去 7 天平均数，CALL 成交量变化最大的股票。
2. 成交量变化最大的 PUT：相对于过去 7 天平均数，PUT 成交量变化最大的股票。
3. 权利金变化最大的 CALL：相对于过去 7 天平均数，CALL 权利金变化最大的股票。
4. 权利金变化最大的 PUT：相对于过去 7 天平均数，PUT 权利金变化最大的股票。

观察这几个数据，从中挑选出你要交易的期权标的。

### 最大流量的期权

![](https://img.forecho.com/5iQCbT.png)

大流量期权数据显示极其庞大的期权订单，鼠标悬停在图表上，可以看到具体的数据。

这有时可以展示聪明资金、对冲或内部知情者的行为。


### 便宜的期权

![](https://img.forecho.com/VqDXmR.png)

钱不够多，但是想交易期权？这里有一些价格低但交易量大的期权。如果你想避免时间价值损失，可以选择 LEAPs(长期股票期权) 。

1. 临近到期的高交易量期权：这些期权通常很快到期，这种期权时间价值损失很快。
2. 到期日在几个月后的高交易量期权：这些期权的时间价值损失很慢，因为到期日很远。

## 分析具体的期权

### 期权交易概况

使用 [Options Dashboard](https://tradytics.com/options-dashboard?ticker=TSLA) 的搜索功能查看具体股票期权交易情况和概况。

![](https://img.forecho.com/jkw7S7.png)

1. 每日保证金热力图：TSLA 热力图展示了在顶级行权价和到期日上花费的每日净保证金。
2. 每周保证金热力图：TSLA 热力图展示了在顶级行权价和到期日上花费的每周净保证金。
3. 历史净保证金：TSLA 每日看涨和看跌期权的净保证金。净保证金是买入头寸支出的保证金减去卖出头寸支出的保证金。
4.净流量：历史净流量显示累计的看涨和看跌期权保证金，是从算法流派中得出的。目前显示 1 天的净流量。

### 成交量和持仓量热力图

![](https://img.forecho.com/wyxq6z.png)

TSLA 在各个行权价和到期日的成交量和未平仓合约指标。当前选择为净值，即看涨期权未平仓合约数减去看跌期权未平仓合约数。

图片右上角可以选择其他指标以及筛选条件。

### 具体期权合约

![](https://img.forecho.com/tTM0AK.png)

1. 最大的订单：按总权利金支出计算，TSLA 的最大订单。鼠标悬停在图表上，可以看到具体的数据。
2. 不寻常的合约：在成交量高于持仓量方面最不寻常的订单。鼠标悬停在图表上，可以看到具体的数据。
3. 成交量最大的 CALLS：按成交量和已支付保证金计算的 TSLA CALL 中的最大期权 - 仅考虑购买头寸。
4. 成交量最大的 PUTS：按成交量和已支付保证金计算的 TSLA PUT 中的最大期权 - 仅考虑购买头寸。

最后，这里只列举了部份，其他的数据可以去 [Options Dashboard](https://tradytics.com/options-dashboard?ticker=TSLA) 自行查看。

## Bullseye

[Bullseye](https://tradytics.com/bullseye) 是基于人工智能的直接预测期权合约价格是否会上涨或下跌。

![](https://img.forecho.com/tJCSb3.png)

1. 自警报发布以来，合同价格变化了多少？

从图中我们看出，其实胜率不高，需要待观察。

## 买卖时机

![](https://img.forecho.com/Mr04X4.png)

Tradytics 的 [Stocks Dashboard](https://tradytics.com/stocks-dashboard) 拥有一个买卖点算法引擎，使用移动平均线和自动生成的支撑阻力线来建议买入和卖出的时机。


更多可以参考[《用 Tradytics 日交易》](https://blog.forecho.com/tradytics-for-day-trading.html) 这篇文章找到支撑和阻力线。根据这些线，我们可以找到买卖时机。

## 最后

Tradytics 是一个非常好的工具，可以帮助我们更好的分析期权和股票。但是，我们还是要注意，这些数据只是辅助，不是决定性的，我们还是要结合自己的分析和判断。也不要因为一个指标而做出决定，要结合多个指标，才能更好的分析。