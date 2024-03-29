---
title: "像专业人士一样交易期权之如何使用 Optionstrat"
date: 2023-01-14T08:47:00+08:00
tags: ["深度理财", "期权"]
draft: false
toc: true
---

![](https://img.forecho.com/81rCn8.png)

## 引言

前几天分享了一篇文章 - [《读懂期权大单》](https://blog.forecho.com/understand-big-option-orders.html) ，介绍了几个可以看期权流的网站，其中一个是 [Optionstrat](https://optionstrat.com/caizhenghai) ，用了一个星期后，今天来分享一些使用心得。

本篇文章适合有一定期权基础的读者，如果你还不了解期权，可以先看看我的 [期权系列文章](https://blog.forecho.com/tags/期权.html) 。

<!--more-->

## Optionstrat 是什么？

[Optionstrat](https://optionstrat.com/caizhenghai) 网站的介绍是：

> 期权交易者的工具包，使用 OptionStrat 直观的可视化和分析工具进行更智能的交易

## Optionstrat 能干什么？

[Optionstrat](https://optionstrat.com/caizhenghai) 主要有三个功能：

- 期权交易策略分析
- 期权优化
- 大单和不寻常的期权交易记录

下面分别介绍一下。

## 大单和不寻常的期权交易记录

这里我们先介绍这个功能，因为这个功能是 Optionstrat 最有用的功能。

![](https://img.forecho.com/1olwyy.png)

这个功能可以看到期权的大单和不寻常的交易记录，包含：

- 期权的大单的概况
- 期权的大单的详细记录（包括不寻常的交易记录）
- 期权的大单的历史记录

### 期权大单的概况

![](https://img.forecho.com/fPIr8P.jpg)

上图是一个期权概要的图，按照交易量排序，左边显示看涨，右边显示看跌的交易量。

可以通过这个页面找到当天严重看涨或看跌的股票，点击股票代码可以看到这个股票的详细交易记录。

⚠️ 注意：

- 看涨的流量表明交易者押注于股票上涨，而看跌则意味着他们押注于股票下跌。可以拿来作为你交易想法的参考。
- 免费版用户数据延迟 15 分钟，付费版用户可以看实时数据。

### 期权大单的详细记录

![](https://img.forecho.com/kTYoPU.jpg)

每一行代表一笔大的交易/不寻常的交易，颜色代表交易方向，绿色代表看涨，红色代表看跌。颜色的深浅有不同的含义，这个他们[官方教程](https://optionstrat.com/tutorials/unusual-options-flow?ref=caizhenghai)有写：

![](https://img.forecho.com/5i0jJa.png)

上面截图是为了方便大家看颜色。另外为了方便大家理解，我这里再解释（翻译）一下：

- `Bullish` (绿色)：看涨交易
- `Bearish` (红色)：看跌交易
- `Very Bullish` (深绿色)：非常看涨交易
- `Very Bearish` (深红色)：非常看跌交易
- `Neutral` (白色)：中性交易，看横盘
- `Directional` (紫色)：方向性交易，预期暴涨或暴跌

OptionStrat 可以识别每笔期权交易的类型：

- `Signle`（白色）：单笔交易，正常的交易
- `Sweep`（橙色）：扫单交易，一次性在多个交易所下多笔交易，试图隐藏自己的交易意图
- `Split`（蓝色）：分割交易，与 Sweep 交易类似，是指在**一个交易所**下多笔交易，大单拆小单。
- `Block`（紫色）：私下协商的订单，在交易所外交易。像拆分和扫荡一样，它们通常被分解成较小的订单，一旦合并就会增加。

### 期权大单的记录详情

点击交易记录可以看到这笔交易的详细信息：

![](https://img.forecho.com/GGznRc.png)

这里简单说明一下这些字段的含义：

- `ACTION`：交易方向，`Buy` 代表看涨，`Sell` 代表看跌
- `OPTION`：期权合约
- `QUANTITY`：交易数量
- `PREMIUM`：期权的借贷成本，为`期权价格*交易数量*100`
- `PRICE`：期权的交易价格，期权的当前价格也会显示在执行价格的下方
    - 如果价格是在卖出价或买入价，`A`或`B`将显示在价格旁边。
    - 如果价格高于卖出价或低于买入价，将显示 `AA` 或 `BB`，表明该订单是非常积极的，交易者优先考虑速度而不是价格。
- `SPOT`：标的价格
- `VOLUME/OI`：显示交易量（今天交易的合同数）和未平仓合约（未平仓合约数，每天更新）。当交易量大于未平仓合约时，这可能表明今天有不寻常数量的头寸被打开。
- `SENTIMENT`：交易的情绪，上文有提到。
- `FLOW TYPE`：交易的类型，上文有提到。
- `Performance`(付费会员功能)：显示交易的当前利润或损失，以及达到的最高和最低点。它还显示交易业绩的历史图表。
- `Calculations`：显示交易的收益和风险计算，关键数据只对付费会员开放。
- `Open in Visualizer`: 点击可以在 Optionstrat 的可视化工具中查看这笔交易的详情，方便。


### 期权大单的筛选

![](https://img.forecho.com/QOzaAj.png)

首页的期权概况右边有筛选功能，你可以根据你的需求筛选出你想要的期权合约，有时候我会过滤掉卖方的期权记录。

另外图上右上角有一个仪表盘图，可以看筛选条件下的期权的情绪，我觉得这个还挺有用的。

Optionstrat 默认给了三个筛选条件，免费使用，很给力，分别是：

- `YOLOs`：YOLO 是 You Only Live Once 的缩写，意思是只有一次机会，所以这里的交易都是高风险的交易，就当买彩票了。过期时间小于 3 天，只看买单，只看暴涨或者暴跌的期权合约。
- `Insiders`：内幕交易，一些可疑的交易，总有人知道些什么。过期时间小于 14 天，只看买单，Volume > OI，只看小单，只看暴涨或者暴跌的期权合约。
- `Highly Unusual`: 非常不寻常的交易。交易量大，Volume > OI.

另外付费会员可以保存筛选条件，保存的时候可以设置是否通知，这个对小众的期权合约很有用。

### 其他

- 付费会员用户才可以看历史的期权数据，免费用户只能看当天的数据。
- 特斯拉真是散户最爱，期权大量被交易，而且交易量大，感觉比较难追踪到交易信号。

## 期权交易策略分析

这个功能是 Optionstrat 的核心功能，可以用来分析期权交易策略的收益和风险。这个功能有的券商有，有的券商没有，有的券商做的好，有的做的很差，但是 Optionstrat 做的非常好。

这个功能对组合期权尤为重要，官方教程 [Options Builder Tutorial](https://optionstrat.com/tutorials/options-builder?ref=caizhenghai)里有详细说明，所以我这里只简单介绍一下。

![](https://img.forecho.com/A2tCmF.png)

### 选策略

用这个之前你要先决定你的策略，最简单的开始就是看涨 `Long Call` 和 `Long Put` 了，另外也可以可以从期权列表的详情页进入这个页面，跟单的时候更方便。

### 输入标的和过期时间

上图显示：

- 策略： `Bull Call Spread` [牛市看涨价差](https://blog.forecho.com/financedeep-33.html#%E7%89%9B%E5%B8%82%E7%9C%8B%E6%B6%A8%E4%BB%B7%E5%B7%AEbull-call-spread%E5%B0%8F%E5%B9%85%E7%9C%8B%E6%B6%A8)
- 标的： `XBI` SPDR 标普生物科技 ETF
- 过期时间： `2023-02-17`
- 两张期权：
    - 买入 `XBI 2023-02-17 89 CALL` 期权
    - 卖出 `XBI 2023-02-17 93 CALL` 期权
- 两个一起买入价格是 87

上图中间的表格横坐标和纵坐标分别是时间和标的价格，中间是理想价格，当然中间的时间你可以点表格按钮进行切换，最常用的是选价格，利润/亏损的金额，利润/亏损百分比，很直观。

另外图上 `89C` 和 `93C` 的按钮是可以点的，可以修改你的成交价格，默认是此刻的标的价格。可以保存你的交易计划，OptionStrat 会显示你的盈亏。

![](https://img.forecho.com/eUV61D.png)

如上图显示，你可以点击圈圈按钮，给你的交易计划设置目标，打到目标就可以平仓了。

另外期权的价格很很多因素有关系，其中最大的关系莫过于标的价格，到期时间，隐含波动率，这些都是你要考虑的因素。

## 期权优化

这个是 [Optionstrat](https://optionstrat.com/caizhenghai) 的一个高级功能，可以用来优化期权组合的收益和风险。

目前我用这个功能不对，有兴趣你们自己去看官方教程 [Strategy Optimizer Tutorial](https://optionstrat.com/tutorials/strategy-optimizer?ref=caizhenghai)。

## 使用经验

### 使用流程

先去 [Flow Summary](https://optionstrat.com/flow?ref=caizhenghai) 页面看期权概况，然后找熟悉的标的关注，然后点进去看交易记录，观察可疑的交易，目前主要是看买单。

然后去券商 App 看对应股票的走势，看对应期权的走势，成交量等等因素。

另外还要考虑大盘的走势，最后才决定要不要跟单。

![](https://img.forecho.com/g7fyXY.png)

本月 4 号开始使用，11 号开始付费，现在收益率已经 35%，目前唯一不确定的是「这笔钱是靠运气赚到的还是靠我自己分析赚到的」，有待时间去检验我的成果 😄。

### 心得

另外分享一下我的使用心得：

- 期权要找你熟悉的标的，这样你才能知道它的股性，心里有底。
- 选流动性好的股票做期权，不然买卖价格差太多，赚不到什么钱。中概股期权流动性往往不好。不知道选哪个就专注于做 SPY 吧。
- YOLOs 或者末日期权少做，成功率太低。
- 买期权容易，卖期权才是最难的，我目前的策略就是有一定的利润就走了，卖飞了也心痛，但是也没办法，总比亏钱好，目前我还在测试几种卖期权的策略，后面有效果会分享出来。

刚开始几天我也是用的免费帐户，数据延迟 15 分钟，凑合着用也可以。但用了几天后发觉实时数据会更好，15 分钟对于期权来说波动性挺大的，然后就付费了，年付的话 $41.67/月，如果胜率很高的话，这个钱还是值得的。

付费可以免费试用 7 天，如果你觉得不好用，可以取消订阅，不扣钱。iOS 端比网页端更便宜一点。

Tip: 虽然他们有限制登录的设备数量，但是还是允许 2 个设备同时登录的，所以你可以找一个朋友拼单，一起用，这样就可以 5 折了。

## 总结

不同于股票，交易期权还是要靠很多数据支撑的，期权的价格都是计算的，不能靠感觉交易，不然很容易亏钱。

[Optionstrat](https://optionstrat.com/caizhenghai) 是一个很不错的期权交易工具，它能让更快的找到交易的机会，让你像专业人士一样交易期权。

免费版足够良心，我看有些期权平台，数据都延迟 3 天，3 天之前的数据有啥用？

如果你还没接触期权，建议你一定要先了解期权的风险再决定是否入坑，毕竟学的更多，亏的也更多 😂。

## 福利

- Telegram 交流群：欢迎加入我的 Telegram 交流群，一起学习交流：<https://t.me/BaoFuTogether>
- 开户福利：如果你还没有美股开户的话，推荐我正在使用的长桥证券，他们目前开户成功入金有送股票，开户链接：<https://link.3li3.com/qiao>

![长桥证券开户](https://blog-1251237404.cos.ap-guangzhou.myqcloud.com/202302103dIO3V.png!s)
