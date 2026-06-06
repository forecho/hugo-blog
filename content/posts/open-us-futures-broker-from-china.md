---
title: "大陆用户开美国期货账户，我会先选 AMP Futures"
date: 2026-06-07T00:36:07+08:00
tags: ["深度理财", "期货", "开户"]
draft: false
toc: true
---

## 引言

最近在研究美国期货账户，主要看了两个券商：[AMP Futures](https://www.ampfutures.com/) 和 [Optimus Futures](https://optimusfutures.com/)。

我的判断很直接：**先开 AMP，Optimus 先放观察位**。

AMP 的开户资料更多，中文圈用的人也更多，Mac 用户可以直接接 TradingView。Optimus 官方支持海外用户，也有多个 FCM 清算商可选，但是中文圈完整跑通开户、入金、交易、出金的人太少，我会把它当第二账户备选。

<!--more-->

## 开户前准备

大陆用户开户，先把这几样材料准备好：

- 护照：最省事的身份证明。
- 地址证明：AMP 可以先准备最近 90 天内的银行账单、水电煤账单、宽带账单。Optimus 这次邮件反馈要求提供银行账单以外的地址证明。
- 海外手机号：AMP 注册会发短信验证码。
- 海外银行账户：后面美元电汇会更稳。

AMP 官方写的地址证明口径比较细，账单上要有姓名、地址、日期。我的建议是申请表地址直接照着账单写，英文地址顺序、房号、楼层都保持一致。Optimus 这边要提前准备水电煤、宽带、有线电视账单，或者其他他们认可的政府/保险类文件。

## AMP 注册的坑

AMP 注册入口是 [AMP Client Portal](https://www.ampclientportal.com/)。

我注册的时候遇到的第一个坑就是手机号验证。大陆手机号收不到验证码，Google Voice 可以收到。

注册成功后，马上去账户安全里打开两步验证。这样后面登录主要走验证器，短信验证码的重要性会下降很多。

开户流程本身很常规：

1. 填个人资料。
2. 填财务情况和交易经验。
3. 上传护照和地址证明。
4. 等审核。
5. 入金。
6. 选择平台、数据源和行情权限。

AMP 官方 FAQ 写的最低入金是 $100。这个金额只适合激活账户，真正交易要看保证金和回撤承受能力。比如 AMP 当前页面显示，MES 日内保证金 $40，MNQ 日内保证金 $100，ES 日内保证金 $400，NQ 日内保证金 $1,000。

我会从 MES 或 MNQ 这种微型合约开始，账户至少留出几倍保证金缓冲。期货亏钱速度很快，刚开始用最小合约熟悉下单、止损、行情和手续费。

## Pending Additional Documents 怎么处理

我现在遇到的状态是 `Pending Additional Documents`。

我上传的是香港汇丰的月结单，里面已经是英文地址，理论上符合银行账单作为地址证明的口径。但是状态仍然卡在补件，这种情况先找客服问清楚后台备注，再决定补哪份材料。

有时差的情况下，我会优先发邮件。邮件可以把问题、附件、账户信息一次说清楚，也方便留记录。

AMP 官方客服邮箱是：`support@ampfutures.com`。

邮件标题可以这样写：

```text
Application Pending Additional Documents - Proof of Address Uploaded
```

正文可以直接用：

```text
Hi AMP Support,

My application status is currently “Pending Additional Documents”.

I uploaded my HSBC Hong Kong bank statement as proof of address. The statement includes my full name, full English address, and was issued within the last 90 days.

Could you please confirm exactly what additional document is required, or what issue you found with the uploaded proof of address?

Application email: 你的注册邮箱
Full name: 你的护照英文名

Thank you.
```

附件我会一起带上：

- 香港汇丰月结单 PDF。
- 护照照片页。
- AMP 后台 `Pending Additional Documents` 截图。

发件邮箱用 AMP 注册邮箱。重点是让客服明确说出缺哪项材料，或者这份地址证明哪里不符合要求。拿到明确回复后再补材料，效率最高。

## AMP 问收入和财富来源怎么回

后面 AMP 可能会继续追问就业状态、收入来源和财富来源。

这里要如实回答，重点是口径一致。当前状态可以写 `unemployed / between jobs`、`employed` 或 `self-employed`；资金来源可以写 `personal savings from previous salary income`、`salary income` 或 `business income`。

这里还有一个小插曲：我填表的时候把就业状态写成了退休，后面客服就追问相关信息。实际状态是待业，就直接写 `unemployed / between jobs`。就业状态和资金来源保持简单、真实、一致，重点是说明交易资金来自本人。

## Mac 用户怎么用 AMP

Mac 用户最简单的方式是：**AMP + CQG + TradingView**。

大致路径是：

1. AMP 开户审核通过。
2. 在 AMP 后台选择 CQG 数据源。
3. 开通 CME 行情权限。
4. 拿到 CQG 登录信息。
5. 打开 TradingView 的 Trading Panel。
6. 选择 AMP。
7. 用 CQG 账号登录。
8. 在 TradingView 里确认行情和下单都走 AMP 连接。

TradingView 网页版和桌面版都可以用，Mac 上体验最省心。

Quantower 也支持 AMP，但是官方桌面端当前主要是 Windows。Mac 用户为了 Quantower 去折腾虚拟机意义不大，TradingView 先跑通最实际。

## TradingView 会员和实时行情

这里很容易搞混：**TradingView 会员解决图表功能和刷新频率，CME 实时行情要单独开通**。

TradingView 官方说明里写得很清楚，交易所实时行情要单独订阅，或者通过券商账户验证。AMP 的说明也写到，TradingView 需要付费计划才能用 AMP/CQG 的实时数据，券商验证有效期一般是 7 天，到期后要重新验证。

所以你已经充值了 TradingView 会员，接下来还要看三件事：

- AMP 后台是否开通了 CME 实时行情。
- TradingView 是否已经连接 AMP/CQG。
- 图表上看的 symbol 是否来自可交易的 AMP/CQG 数据源。

看 TradingView 自己的延迟行情时，会员也会看到延迟。连上 AMP/CQG，并且开了 CME 实时行情后，TradingView 图表和下单面板才会走实时数据。

还有一个细节：`MES1!`、`MNQ1!` 这种连续合约适合看图，真实下单要用具体月份合约，比如 `MESM2026`、`MESU2026` 这种。下单前看一眼 Trading Panel 里的报价来源，别用错 symbol。

## Optimus 我为什么先观望

Optimus 是介绍经纪商，后面接 Ironbeam、ADMIS、StoneX、Phillip Capital、Wedbush 等 FCM。它的优势是客服和平台引导，官方也支持海外用户。

我担心的是中文圈案例太少。开户能通过是一回事，后面还有入金、行情、平台、出金、税表、客服沟通。期货账户这种东西，我更愿意选择公开经验多一点的路径。

我这次也遇到了一个具体问题：一开始用银行账单做地址证明，Optimus 邮件反馈要求提供银行账单以外的地址证明。他们列出来的材料包括水电费账单、燃气账单、有线电视账单、有效驾照、政府签发身份证、汽车保险单等。

![](https://r2.imgant.com/2026/06/04/8c311f005c535366c58975db909dc33d.png)

我的情况比较尴尬：目前手上没有这类地址证明，所以 Optimus 开户暂时办不下去。这也是我会先选 AMP 的一个现实原因。

Optimus 当前 FAQ 写的最低入金是：微型期货 $500，迷你或标准合约 $2,000。手续费页面写的是微型合约 $0.25/side，标准合约 $0.75/side，非美国客户可能会有额外清算费。

这个账户可以放在第二阶段考虑：等 AMP 跑通后，再开 Optimus 对比平台、手续费和客服体验。

## 费用怎么看

美国期货费用要拆开看：

`单边成本 = 经纪商佣金 + 交易所费 + NFA 费 + 清算费 + 平台/路由费`

`月度固定成本 = 行情数据费 + 数据源连接费 + 平台月费`

AMP 的 CME 四大交易所 Level 1 打包行情当前是 $15/月，单个 CME/CBOT/NYMEX/COMEX Level 1 是 $5/月。Level 1 对看图和普通下单够用，Level 2 主要给看盘口深度的人。

出金也要看。AMP 国际电汇出金 $30，Optimus 的费用跟你选择的 FCM 相关。

我会先把成本控制简单：

- 只开自己交易的交易所行情。
- 先用 TradingView。
- 先交易微型合约。
- 先把入金和出金跑通。

## 我的选择

第一阶段我会开 AMP。理由是资料多、门槛低、Mac 上接 TradingView 方便，Google Voice 可以解决注册验证码。

Optimus 放到第二阶段。等 AMP 的开户、入金、实时行情、下单、出金都跑通后，再决定是否多开一个 Optimus 账户。

真正开始交易前，先用微型合约把流程练熟。每次下单前先算最大亏损，账户能活下来，后面的策略才有意义。

## 参考链接

- [AMP Futures：开户流程](https://faq.ampfutures.com/hc/en-us/articles/10796123577239-How-do-I-apply-for-an-AMP-Futures-trading-account)
- [AMP Futures：开户材料](https://faq.ampfutures.com/hc/en-us/articles/10797239636759-What-documents-are-required-for-account-verification)
- [AMP Futures：最低入金](https://faq.ampfutures.com/hc/en-us/articles/33479414576279-Is-there-a-minimum-capital-requirement-to-open-an-account)
- [AMP Futures：保证金](https://www.ampfutures.com/trading-info/margins)
- [AMP Futures：行情数据费](https://www.ampfutures.com/trading-info/exchange-data-fees)
- [AMP Futures：客服联系方式](https://www.ampfutures.com/contact-us)
- [AMP Futures：TradingView 延迟行情说明](https://faq.ampfutures.com/hc/en-us/articles/35688297911831-TradingView-Delayed-Data)
- [TradingView：用券商账户验证实时行情](https://www.tradingview.com/support/solutions/43000479666-how-can-i-get-real-time-data-from-exchanges-that-i-have-already-purchased-with-my-broker/)
- [TradingView：实时行情来源对交易的影响](https://www.tradingview.com/support/solutions/43000739323-how-does-the-source-of-real-time-data-affect-the-trading-experience/)
- [TradingView：CME 期货行情说明](https://www.tradingview.com/cme/)
- [Optimus Futures：海外客户开户](https://support.optimusfutures.com/can-i-open-an-account-if-i-live-outside-the-united-states)
- [Optimus Futures：最低入金](https://support.optimusfutures.com/what-is-the-minimum-initial-deposit-required-to-open-an-account)
- [Optimus Futures：手续费](https://optimusfutures.com/Futures-Trading-Pricing.php)
