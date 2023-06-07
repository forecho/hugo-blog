---
title: "股票技术指标 - 移动平均线（MA）指标"
date: 2023-05-13T10:49:00+08:00
tags: ["深度理财", "股票技术指标"]
draft: false
toc: true
---

## 引言

MA 是股票技术分析中最常用的技术指标之一，经常被用来判断股票的趋势。这篇文章主要介绍 MA 指标的计算方法和使用方法。

## 什么是 MA 指标

MA 是移动平均线 (Moving Average) 的缩写，简称「均线」，代表过去一段时间内股票的平均价格。


## 计算方法

均线的计算方法相对简单。它是在一定时期内股票收盘价的平均值。

以下是计算均线的基本步骤：

<!--more-->

1. **选择一个时间段**：这个时间段可以是任何你选择的连续天数。常用的有 5 日、10 日、20 日、60 日、120 日、240 日等。

2. **加总这个时间段内的收盘价**：例如，如果你正在计算 10 日均线，你就需要把过去 10 天的收盘价加总起来。

3. **除以天数**：然后，你把步骤 2 中得到的总和除以你选择的天数。例如，如果你在计算 10 日均线，你就需要把总和除以 10。

这个结果就是你选择的时间段内的均线值。

例如，计算 5 日均线的公式为：

```
MA5 = (P1 + P2 + P3 + P4 + P5) / 5
```

其中，P1、P2、P3、P4、P5 分别代表最近 5 天的收盘价。

需要注意的是，均线每天都在变动，因为每天都有新的收盘价加入计算，同时最早的收盘价被排除出去。

## 使用目的

均线的主要目的是帮助投资者理解股票价格的趋势。它可以提供以下几种重要信息：

### 趋势判断

当股票价格持续高于某一均线，特别是长期均线，如 50 日或 200 日均线，就可以认为股票可能处于上升趋势中。相反，如果股票价格持续低于某一均线，就可能处于下降趋势中。

### 买卖信号

![](https://img.forecho.com/fyEDGV.png)

短期均线和长期均线的交叉通常被视为买卖信号：

- **买入信号**：短期均线（如 5 日或 10 日均线）从下方穿过长期均线（如 50 日或 200 日均线），这被视为「金叉」。
- **卖出信号**：反之，如果短期均线从上方穿过长期均线，这被称为「死叉」。

### 支撑和阻力

**支撑**：均线有时也被视为可能的支撑或阻力水平。如果股票价格下跌到某个均线，并在那里停止下跌或反弹，那么这个均线可能就是一个支撑线。

**阻力**：相反，如果股票价格上升到某个均线，并在那里停止上涨或开始回落，那么这个均线可能就是一个阻力线。

### 价格与均线的偏离程度

价格与均线的偏离程度也能提供一些信息。

- 如果股票价格远高于均线，可能表示过度买入，需要警惕价格回落的风险；
- 如果股票价格远低于均线，可能表示过度卖出，有可能会有反弹的机会。

## 使用方法

### 简介

常用的均线有以下几种：

-  5 日均线（MA5），即周线
- 10 日均线（MA10），即半月线
- 20 日均线（MA20），即月线，非常主要的均线
- 60 日均线（MA60），即季线
- 120 日均线（MA120），即半年线
- 240 日均线（MA240），即年线，也有人用 250 日均线代替年线

根据你的风险偏好和投资周期，你可以选择不同的均线来判断股票的趋势，策略如下：

| 策略 | 超短线 | 短线 | 中线 | 长线 | 
| ---- | ---- | ---- | ---- | ---- |
| 交易时间 | 日内 | 几天 | 几周 | 几个月 |  
| 使用均线 | 1 分、5 分、16 分、60 分  | 3 日、5 日  | 20 日、60 日  | 120 日、240 日  |

另外：

- 月线是最重要的均线，是短线操作的多空分界线
- 季线是中长期操作的多空分界线
- 趋势多头在月线之上，月线上扬，做多；趋势空头在月线之下，月线下跌，做空

### 单一均线交易法

一周左右的短线我们主要看 5 日、10 日、20 日均线。单一均线交易法是指我们选一个均线，比如 5 日均线，然后根据这个均线的走势来判断买卖。

![](https://img.forecho.com/S0N9h3.png)

看一下中国科传的例子：

- 在上图 1 的位置之前三条均线很乱，不建议入场。回撤但是 MA5 一直在 MA10、MA20 之上，这时候是一个很不错的入场点。
- 当 MA5、MA10、MA20 三条均线都向上，且 MA5 在 MA10、MA20 之上，且 MA10 在 MA20 之上，这时候可以入场，建议在收盘前 10 分钟左右入场。新手不建议在盘中操作。
- 止损点：把**进场当日股价的最低价**设置为止损点，如果股价跌破这个点，就卖出。
- 每天收盘前 10 分钟左右检查一下，没有跌破止损点就继续持有，直到出现卖出信号。
- 卖出信号：收盘前 10 分钟左右，股价跌破均线就卖出。如果你是 MA5 进场的就看 MA5，如果你是 MA10 进场的就看 MA10，依此类推。MA5 太累了，所以我推荐 MA10。

#### 原则：

- 此方式只适用于多空趋势明显的时候（当 MA5、MA10、MA20 三条均线都向上，且 MA5 在 MA10、MA20 之上，且 MA10 在 MA20 之上我们称之为多头趋势），不适合震荡期间。
- MA20 均线向上时，适合做多，MA20 均线向下时，适合做空。
- 无论是做多还是做空，都要以**快收盘价**为准，即在收盘前 10 分钟左右，手动操作，防止盘中出现大幅波动。
- 日线和周线皆可操作。
- 入场和出场看同一个均线，比如入场看 MA5，出场也看 MA5。
- 均线选择：
    - MA5：短线操作，一周左右
    - MA10：中短线操作，一周到一个月
    - MA20：中线操作，一个月左右

## 真假金叉

![](https://img.forecho.com/lWCRCG.png)

我们知道短均线（MA5、MA10）从下往上穿过长均线（MA20、MA60）时，称之为金叉，是买入信号；反之，称之为死叉，是卖出信号。

如上图 1 所示，MA5 从下往上穿过 MA20，股价也随之上涨，这是一个典型的金叉买入信号。

但是有时候并不是这样的，比如上图 2 的位置，MA5 从下往上穿过 MA20，但是股价并没有上涨，反而下跌了。因为此时**长均线 MA20 仍然向下**，这种情况我们称之为假金叉，不可盲目买入。

## 注意事项

需要注意的是，均线是延后指标，意味着它基于过去的数据。因此，它不能预测未来价格，只能提供对当前市场趋势的理解。

同时，任何技术指标都不能保证 100% 的准确性，因此在使用均线时，投资者还需要结合其他技术指标和市场信息。以下是一些可能导致均线失效的情况：

### 震荡市场

在震荡或横盘的市场中，股票价格可能会频繁地在均线上下穿越，导致均线生成的买卖信号失效。在这种市场环境中，均线往往会产生许多假信号，使得投资者反复买入和卖出，增加交易成本。

### 市场新闻或突发事件

重大的市场新闻或突发事件，如公司的财报发布、突然的经济政策变动、大规模的市场恐慌等，都可能导致股票价格短时间内大幅波动，这种情况下，均线可能无法及时反应这些变化。

### 短期极端价格波动

如果股票价格在短期内发生极端波动，比如由于市场操纵或其他非常规行为导致的大幅度价格波动，那么均线也可能失效，因为它是基于过去一段时间的平均价格计算的，可能无法反映出这种短期内的极端变动。

## 总结

均线是一种简单而有效的技术指标，它可以帮助投资者更好地理解市场趋势，从而做出更好的投资决策。在使用均线时，投资者需要注意以下几点：

- 均线是一种趋势指标，可以帮助投资者更好地理解市场趋势。
- 均线是一种延后指标，不能预测未来价格。
- 均线的选择需要根据投资者的投资目标和投资周期来确定。
- 均线的使用需要结合其他技术指标和市场信息。
- 均线在震荡市场、市场新闻或突发事件、短期极端价格波动等情况下可能失效。

如何你看完本篇文章有任何疑问或者建议，欢迎在下方留言，我会及时回复。