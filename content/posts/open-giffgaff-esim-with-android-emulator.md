---
title: "用安卓模拟器开通 giffgaff eSIM 的记录"
date: 2026-06-15T16:58:35+08:00
tags: ["经验分享", "eSIM", "giffgaff"]
draft: false
toc: true
---

![](https://r2.imgant.com/2026/06/15/b58745b7.png)

## 引言

6 月 14 日晚上，看到几条推文和一个 YouTube 教程后，我跟着折腾了一圈，最后用电脑上的安卓模拟器申请到了英国 giffgaff eSIM。

我的主力机只有一台 iPhone 16 Pro Max，双实体卡槽。一个卡槽放老号，用来收短信和接电话；另一个卡槽放宽带送的流量卡，日常上网。第三方 eSIM 实体卡要插进 iPhone，就要占一个卡槽。

这次先把 giffgaff eSIM 申请下来，等 eSTK Plus+ 到货后再写入实体卡使用。Max 当时缺货，Plus+ 对我这个场景已经够用。

<!--more-->

## 我的需求

我需要一个海外手机号，主要用来收验证码、注册海外服务，以及做账号备用。giffgaff 的优势是英国号码、支持 eSIM、维护成本低，中文圈也有不少实操记录。

我这次已经在 giffgaff 下单并拿到了 eSIM 信息，后面的重点是把这个 eSIM profile 写进 eSTK，再把 eSTK 当实体卡插进 iPhone。

## 我一开始想用旧安卓手机

我手上有一台十多年前的安卓手机，最开始想通过云萝卜这类工具配合 HookEuicc 来搞定。

实际卡在了系统版本上。老安卓系统太旧，HookEuicc 一直安装失败，后面的 LSPosed、模块生效、giffgaff App 检测都走不下去。

这个坑比较典型。教程里写“准备一台安卓手机”，真正执行时还要看系统版本、Magisk、LSPosed、HookEuicc 这些环节。我的旧手机第一步就卡住了，换电脑安卓模拟器更省时间。

## 最后跑通的是电脑安卓模拟器

我参考的 YouTube 教程标题很直接：《无需手机，全电脑操作：英国 giffgaff 电话卡手把手申请教程》。视频里走的是电脑安卓模拟器路线，核心工具大致是：

- MuMu 模拟器
- Magisk
- LSPosed
- HookEuicc / 相关 giffgaff 模块
- giffgaff App

大致流程是：

1. 在电脑上安装安卓模拟器。
2. 安装 Magisk，确认 root 环境正常。
3. 安装 LSPosed，并启用 HookEuicc 相关模块。
4. 安装 giffgaff App，登录账号。
5. 在 App 里申请 eSIM。
6. 保存拿到的 eSIM 激活信息。

我就是沿着这条路跑通的。相比旧安卓手机，模拟器环境可控，版本也更容易匹配教程。

一般会看到类似这种格式：

```text
LPA:1$SM-DP+地址$Activation Code
```

这里面的 `SM-DP+地址` 和 `Activation Code` 要保存好。后面写入 eSTK、9eSIM、XeSIM 这类实体 eSIM 卡时都要用到。

## 为什么我最后选 eSTK Plus+

我最开始在 9eSIM、XeSIM、eSTK 之间纠结。调研之后，我觉得 eSTK 性价比最高：

- 价格合适。
- 容量够用，我主要放 giffgaff，再加少量旅行 eSIM。
- iPhone 用户可用，STK 菜单和跨平台工具都能覆盖我的使用方式。

Max 的容量更大，更适合经常切换很多国家和运营商 eSIM 的人。可惜我下单时 Max 没货，所以直接买了 Plus+。

购买链接：[eSTK Plus+](https://store.estk.me/products/1)，优惠码 `UP40YC8KOD` 可以打 9 折。

我的使用方案很简单：

1. eSTK Plus+ 到货后，把 giffgaff eSIM 写进去。
2. iPhone 里继续保留老号，保证短信和电话稳定。
3. 日常国内上网继续用宽带送的流量卡。
4. 需要 giffgaff 或旅行 eSIM 时，把第二卡槽换成 eSTK。

这也是双实体卡 iPhone 的现实取舍。老号承载短信和电话，我会固定留在手机里；第二卡槽在流量卡和 eSTK 之间按场景切换。

## 写号和启用

我现在已经买了 giffgaff eSIM，也拿到了 profile。后面要做的是把它写进 eSTK，再在 iPhone 上启用。

流程大概是这样：

1. eSTK 到货后，把卡插进 iPhone。
2. 进 `设置 > 蜂窝网络 > eSTK 这张卡 > SIM 卡应用程序`。
3. 点 `下载 Profile`。
![](https://r2.imgant.com/2026/06/17/be3736f3.png)
4. 进去后会有两个选项，选第二个。第一个是系统默认。
5. 输入完整的 `LPA:1$SM-DP+地址$ActivationCode`。
6. 等成功提示。返回后再进 `SIM 卡应用程序`，可以看到下载进度。
7. 进度没到 100% 就返回再进来看，直到显示 100%。
8. 回到 `管理 Profile`，启用 giffgaff，前面有 `#` 表示已启动。
9. iPhone 里打开 giffgaff 这条线，蜂窝数据继续选国内流量卡。

这里最容易踩坑的是下载过程。写号需要联网，操作时保持页面不动，等成功提示再退出。中途切后台、锁屏、拔卡、断网，都容易写入失败。

写入成功后，我给 `+44 78020 02606` 发了一条 `hi`，很快收到短信，说明 giffgaff 已经可以正常收短信。

![image](https://r2.imgant.com/2026/06/17/b4b89554.png)

## 注意事项

- eSTK 可以保存多个 eSIM profile，但同一时间只能启用一个。
- 写好 profile 后，eSTK 可以当实体 SIM 卡插到 Android 手机里用；切换 profile 需要 Android 支持 `SIM 卡应用程序`。
- 关闭“允许切换蜂窝数据”，蜂窝数据继续走国内流量卡。
- 平时关闭 giffgaff 数据漫游，收短信和保号走普通短信。
- iMessage / FaceTime 弹出短信激活收费提示时选取消。
- 启用后等 1-15 分钟，看到 `R` 漫游标识或信号条后再测试收短信。
- 没信号先开关飞行模式，等 15 秒；还没信号再重启手机。
- 继续没信号，进 `设置 > 蜂窝网络 > 网络选择`，关掉自动，优先手动选中国移动。
- 收不到验证码时，先确认账户状态、eSTK profile、iPhone 线路和网络选择。
- 写入后可以给 `+44 78020 02606` 发 `hi` 做短信收发测试。
- 登录 giffgaff 官网或 App 用英国本地格式：`07xxxxxxxxx`。注册其他服务选英国区号 `+44` 后，输入 `7xxxxxxxxx`。
- 有些平台会拦截英国号码或海外号码，这类情况换时间、换网络环境、换服务测试。

## 关闭语音信箱

我在中国漫游状态下直接拨 `1626` 打不通，最后走 giffgaff 的海外语音信箱工单：

1. 打开 [Accessing voicemail while abroad](https://support2.giffgaff.com/app/ask/International-and-Roaming/Accessing-voicemail-while-abroad/form/)。
2. 登录 giffgaff 账号。
3. 提交关闭语音信箱的请求。

留言可以直接写：

```text
Hi giffgaff team,

I am currently roaming in China and cannot call 1626 to turn off voicemail.

Could you please disable voicemail for my giffgaff number?

My giffgaff number: 07xxxxxxxxx

Thank you.
```

## 查看手机号

写入并驻网后，giffgaff 会发短信告诉你手机号，短信里通常是英国本地格式：

```text
07xxxxxxxxx
```

换成国际格式很简单：去掉开头的 `0`，前面加 `+44`：

```text
+44 7xxxxxxxxx
```

登录 giffgaff 官网和 App 用 `07xxxxxxxxx`，注册其他服务选择英国 `+44`，号码填 `7xxxxxxxxx`。

另外登录 giffgaff 网页版，账号首页也能看到手机号。

## 低成本保号

giffgaff 保号按 180 天内产生一次余额变动来执行，比如发短信、打电话、使用移动数据，或者买话费/套餐。

核心就是：每 179 天给 `+44 7973000186` 定时发一条短信，内容随意。中国漫游发短信当前是 0.30 英镑/条，约人民币 3 元，收短信免费。一年两条，成本约 0.60 英镑。

这里要看清楚余额类型。买 eSIM 时花的钱可能买的是 plan，保号短信扣的是 Airtime credit。发短信前先在 giffgaff App 或网页后台看 credit balance。

可以直接用这个快捷指令：

1. 打开这个 iCloud 链接：[giffgaff 保号快捷指令](https://www.icloud.com/shortcuts/b378d5581f474e19ac335d68671d0ed5)。
2. 第一次运行时，确认发送线路是 giffgaff/eSTK。
3. 发送成功后，快捷指令会重新计时，等 179 天后再次发送。

发完之后去 giffgaff App 或用量记录里确认有 0.30 英镑短信扣费记录。如果账户里有 10 英镑 Airtime credit，大约能发 33 条短信，按 179 天一次算，理论上能保号 16 年左右。

充值用 Airtime credit，最低充值金额当前是 10 英镑。保号用短信扣话费，别买月套餐。实体卡常见的充值赠送活动和 eSIM 购买流程分开看。

余额查询有三种方式：拨 `*100#`、登录官网、打开 giffgaff App。保号成功后会有扣费和余额变化，看到余额变动就够了。发给国内号码容易被拦截，保号优先发给 `+44 7973000186` 这种英国号码。

## 参考资料

- [YouTube：无需手机，全电脑操作：英国 giffgaff 电话卡手把手申请教程](https://www.youtube.com/watch?v=0UnJjASaqE8)
- [giffgaff 官方：Switching to an eSIM with giffgaff](https://help.giffgaff.com/en/articles/261570-switching-to-an-esim-with-giffgaff)
- [giffgaff 官方：Understanding why your number has been deactivated](https://help.giffgaff.com/en/articles/242797-understanding-why-your-number-has-been-deactivated)
- [giffgaff 官方：Roaming charges](https://www.giffgaff.com/roaming-charges)
- [giffgaff 官方：Everything to know about Credit](https://help.giffgaff.com/en/articles/240847-everything-to-know-about-credit)
- [Apple：iMessage 和 FaceTime 激活短信说明](https://support.apple.com/en-us/119859)
- [eSTK：ESTKme 下载入口](https://store.estk.me/downloads)
- [X：giffgaff 保号快捷指令](https://x.com/nk912114/status/2010322740931387604)
- [giffgaff 非官方中文网：关闭 voicemail 语音信箱](https://www.giffgaff.shop/qa/07-voicemail-switch/)
- [GitHub：英国实体 SIM 卡 giffgaff 激活使用教程](https://github.com/ssnhd/giffgaff)
