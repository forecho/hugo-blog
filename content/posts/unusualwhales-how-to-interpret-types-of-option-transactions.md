---
title: "Unusual Whales 如何解读期权交易类型"
date: 2025-07-04T22:00:00+08:00
tags: ["期权", "美股", "Unusual Whales"]
draft: false
toc: true
---

## 引言

本文将详细介绍如何判断期权合约的买卖方向，以及如何区分开仓和平仓。内容参考自 [Unusual Whales 官方文档](https://unusualwhales.com/information/how-to-interpret-types-of-option-transactions)。

## 期权交易的四种基本方式

**你可以通过四种方式交易期权合约：**

- 买入看涨期权（买价方向/在买价）
- 卖出看涨期权（卖价方向/在卖价）
- 买入看跌期权（买价方向/在买价）
- 卖出看跌期权（卖价方向/在卖价）

假设你看好 ABC 公司，可以买入看涨期权。平仓时卖出该看涨期权。若你看空 XYZ 公司，可以买入看跌期权，平仓时卖出该看跌期权。

> 你可以观看 Unusual Whales 的 [期权交易的四种方式](https://www.youtube.com/watch?v=9g74rfdEILA) 视频，了解更详细的讲解和实际 Flow Feed 示例。

<!--more-->

## 期权交易的四种类型

1. 买入看涨期权
2. 卖出看涨期权
3. 买入看跌期权
4. 卖出看跌期权

### 买入合约

- 买入开仓（BTO）
- 买入平仓（BTC）

### 卖出合约

- 卖出开仓（STO）
- 卖出平仓（STC）

"开仓"和"平仓"这两个短语为交易增加了更多含义和背景。每一笔期权交易都会是"开仓"或"平仓"之一。

"买入开仓"指的是交易者为自己买入一个新头寸（无论是看涨还是看跌）。当交易者准备卖出（或平仓）时，就是"卖出平仓"。

"卖出开仓"指的是交易者开一个新头寸，但站在空方。比如卖出（或写）备兑看涨/现金担保看跌就是"卖出开仓"交易。要平掉"卖出开仓"的头寸，你需要"买入平仓"。

如何判断一笔交易更可能是买入还是卖出？看买卖价差的哪一侧。

Side（方向）是你会在Unusual Whales flow feed中看到的一个表头。它通常用来指明一笔交易在买卖价差中的位置。你可以通过阅读[这篇文章](https://unusualwhales.com/information/how-is-the-side-of-the-trade-determined-in-the-flow)了解更多关于"方向"的信息。

![Image 2](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc4uI0Zuxxa-pJ1k13Bp95elu0vKunfRj9idGOVEKskNQ8ySTaAYuqJDswq8Ye2S1kDUZsJHdbAmXOwy6LW-TYkluyGArcO1JQh8x71SgtdJI0nFXlQxWpcltwqGmEwNNBmwXEhAGLCamz7gQ0C_X04aUbE?key=WKASwVLK7h_TndCyb3ltig)
每笔交易都会被标记方向。共有四种可能的方向：

*   **Ask（买价）** - 在买价或更接近买价成交的交易会被标记为ASK。
*   **Bid（卖价）** - 在卖价或更接近卖价成交的交易会被标记为BID。
*   **Mid（中间价）** - 恰好在买卖价差中间成交的交易会被标记为MID。
*   **None（无）** - 前三种标签是指交易在买卖价差中的位置。NONE标签用于买卖价差无关紧要的情况，比如对敲、顺序外或延迟的交易，或被修改或取消的交易。

**需要注意的是：在买价成交并不保证一定是买入，在卖价成交也不保证一定是卖出，只是概率更高而已。后续文章会详细讲解如何判断，但请注意这些知识并不能保证100%准确。**

为什么这些很重要？

理解这些概念对任何想要分析资金流向，尤其是想跟踪"异常期权流"的交易者来说都至关重要。

区分"开仓"和"平仓"交易的难点在于它们并没有明确标注。

比如，一个交易者买入（平仓）100万美元的（备兑）看涨期权，这和另一个交易者买入（开仓）100万美元的看涨期权在表面上看起来很像，但两者的市场情绪却截然不同。

下面我们会举一些"开仓"和"平仓"流动的清晰与不清晰分析的例子。

## 实例解析

### 买入开仓流动

![Image 3](https://lh7-rt.googleusercontent.com/docsz/AD_4nXed67MamR4K9dSnivk2x0h53pa3micvZKSCp0GduF83DNX24RVTMYLI0FEiyQFAm9GU93RzIL59h9JJjftaG51XyroII_SDEZk3vHOYpcIDuYDHOBnAjW0EPvZCfmcHge8s4IbuIsYnVdA7Fe7u3FBE8Tbc?key=WKASwVLK7h_TndCyb3ltig)
这里我们看到多笔订单在买价或接近买价成交。以[$EVA](https://unusualwhales.com/stock/EVA/overview?) $0.5C 03/15/2024为例，该合约的买卖价差为$0.15 - $0.20。右侧显示成交价为$0.20，正好在买价。此外，订单数量为470张，而未平仓合约数为0。因此我们知道这是开仓交易（订单量 > 当日总成交量 + 未平仓合约数）。

下方是接近买价成交的开仓例子。以[$CRON](https://unusualwhales.com/stock/CRON/overview?) $4C 1/16/2024为例，买卖价差为$0.25 - $0.35，成交价为$0.33。虽然不是正好在买价，但距离买价远比卖价近；同样，订单量大于当日总成交量和未平仓合约数之和，因此我们也能判断为开仓。

这两笔交易都可能代表看涨情绪。但请注意，这不是精确科学。在买价或接近买价成交并不保证一定是买入，只是概率更高。

### 卖出开仓流动

![Image 4](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfpRr5lhDZNvm9OrrvcUCbbzvh14E7xUHHeCM4RIFVCubf33s7Pqe1W_1OAygrZ2np5bwiI4fpgkerE7xH9WRtSqTzSg26GpR83o39rS-pqJTOPiJf4BCgP92maRIlda1itEU4AAc4Bvv4Qrmksl4qm8dJF?key=WKASwVLK7h_TndCyb3ltig)
判断"卖出开仓"交易的方法和"买入开仓"类似，只不过我们要找的是在卖价或接近卖价成交的交易。以[$CHWY](https://unusualwhales.com/stock/CHWY/overview?) $21.5P 2/2/2024为例，买卖价差为$2.45 - $2.49，成交价正好在卖价$2.45。订单量为175张，未平仓合约数仅67张，且订单量大于当日总成交量和未平仓合约数之和，因此可以判断为开仓。

再看[$LLY](https://unusualwhales.com/stock/LLY/overview?) $617.5P 2/2/2024，买卖价差为$8.15 - $8.80，成交价为$8.20，虽然不是正好在卖价，但考虑到价差较大，已经很接近卖价。同样，订单量大于当日总成交量和未平仓合约数之和，因此也可以判断为开仓。

### 买入平仓

![Image 5](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdGEVIaUA9yk3ZBtWobSM8lMhvdokNzTfbf_nzkB3BBjldTgMu7xun4vy4rXFA0Y6AqvgG1F3BFy6uBL8z09e8gwZNXMWPIK_fqEDJUw4hzEThx533RfocUUNNgmRPKbpEpNFKKJFBy2YOKotN_n5ZaDEE?key=WKASwVLK7h_TndCyb3ltig)
买入平仓的流动和买入开仓类似，只不过这里需要有未平仓合约——只有存在头寸，才有平仓。以[$AAPL](https://unusualwhales.com/stock/AAPL/overview?) $185P 12/29/2023为例，11/16有1.75万张合约大多在卖价成交，11/17未平仓合约增加近1.6万张。该头寸持有两周，直到12/13我们注意到合约在买价成交，成交量几乎与最初建仓时相同。第二天，12/14，未平仓合约大幅下降，证实了部分头寸被平掉。

### 卖出平仓

![Image 6](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeOkGPqgnnTnixE88TS5WhKRxFPUvCdcpj3JbM5nrwxnOm1tAVT8f1E8Q_mlSuKGeEl53RV7ujF7eymV-9qiZpgG6vEf4No-G6lO35qdazEP_AWbGICUrWd40MAHrikYhVHVOTruPE72WiABd5cOHWaok4?key=WKASwVLK7h_TndCyb3ltig)
卖出平仓和买入平仓一样，需要有未平仓合约。以[$SPR](https://unusualwhales.com/stock/SPR/overview?) $23C 11/17/2023为例，11/10有5066张合约在卖价$1.46成交，而未平仓合约有10078张。5066张的成交量正好在卖价。我们无法在当天确认是否平仓，需等第二天开盘前未平仓合约更新。结果显示，11/13未平仓合约减少了2324张，证实了部分头寸被平掉。


## 相关文章

- [期权合约图表解析](https://unusualwhales.com/information/breaking-down-an-option-contract-chart)
- [如何查看和使用未平仓合约浏览器](https://unusualwhales.com/information/how-to-check-and-use-the-open-interest-explorer)


## 结语

理解期权交易的类型和流向，有助于更好地分析市场情绪和资金流动。希望本文对你有所帮助。

> 原文出处：[How To Interpret Types of Option Transactions - Unusual Whales](https://unusualwhales.com/information/how-to-interpret-types-of-option-transactions)


*非投资建议，仅供学习交流*
