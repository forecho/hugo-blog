---
title: "用 Tradytics 日交易"
date: 2023-03-03T22:18:00+08:00
tags: ["深度理财", "美股", "日交易"]
draft: false
toc: true
---

## 引言

[Tradytics](http://link.3li3.com/tradytics) 是一个美股期权交易平台，提供了很多有用的数据，可以帮助我们日交易。今天就来介绍一下如何使用 Tradytics 日交易。

## 使用 Tradytics 日交易

主要看 TICKER DASHBOARDS 下面的三个 Dashboard，分别是：

### Stocks Dashboard 

主要目的就是添加 GEX 线

![](https://img.forecho.com/JJth2E.png)

<!--more-->

- 找到 Timeframe，切换成 5 分钟
- 找到 Greek Levels，选「Spot GEX」然后选「All」，然后点「Copy to TradingView」获得 GEX 线。说明：
	- 红线代表做市商会卖
	- 绿线代表做市商会买
- 然后使用同样方法获得 1 小时线
- 没必要把所有的线都加上

### Options Dashboard 

期权状态。先**预览整个状况**，绿色代表看涨，红色代表看跌。

![](https://img.forecho.com/HQQR6U.png)


然后查看「OPEN INTEREST BY STRIKES」**了解未平仓期权数（IO）**，注意要选择最近到期的日期（只看末日期权）。绿色未平仓数数量最大的是阻力线，选两个记录下来。

这个数据每日早上盘前更新，所以记得盘前确认一遍。

![](https://img.forecho.com/AdbjNY.png)

**了解做市商仓位**，查看最新的 Deltas 数据，如果：

- delta 是正数说明：机构和散户看空，他们买 PUT 或者卖 CALL
- delta 是负数说明：机构和散户看多，他们买 CALL 或者卖 PUT

![](https://img.forecho.com/xgIJtr.png)

查看做市商的 IO（未平仓期权数），同样选择末日时间，然后获取到 3 个最大的数字，记录下来。

![](https://img.forecho.com/EUWsMx.png)

查看 SPOTGEX，确认 GEX 线，GEX 每日首次更新时间是开盘时间，然后每 2 个小时 tradytics 更新一次，偶尔过来看一下最新的 GEX 线。
 
![](https://img.forecho.com/o1jhtI.png)

### Darkpool Ticker

![](https://img.forecho.com/hNXOfg.png)

暗池数据，展示当前股票一个月以来暗池最大的交易情况：

- 红色代表阻力
- 绿色代表支撑

选 5 分钟和 1 小时的情况，选择几个交易量最大的点位，记录下来。

## 最后

Tradytics 是一个很好的工具，它的很多数据其他平台都没有，至于如何使用这些数据，就看你的能力了，它为你交易决策提供了很多数据，你需要做的就是把这些数据整合起来，然后做出决策。