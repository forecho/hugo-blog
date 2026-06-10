---
title: "Rectangle Pro 使用教程：买了之后才搞懂"
date: 2026-06-10T14:27:48+08:00
tags: ["Mac", "效率工具", "软件"]
draft: false
toc: true
---

## 引言

之前稀里糊涂买了 [Rectangle Pro](https://rectangleapp.com/pro/)，买完之后才发现它界面全是英文，功能也比免费版多很多。放着吃灰有点浪费，所以我花了点时间把官方文档和常用功能摸了一遍。

先说结论：**免费版 Rectangle 已经够大多数人用，Rectangle Pro 的价值主要在 Window Throw、自定义窗口尺寸、工作区布局、Stash 和 Pin Mode。**

这篇文章按英文菜单名来写，打开软件的时候可以直接对照着设置。

<!--more-->

## Rectangle 免费版和 Pro 有什么区别？

[Rectangle](https://github.com/rxhanson/Rectangle) 免费版是一个开源的 Mac 窗口管理工具，核心功能就是把窗口快速移动到左半屏、右半屏、最大化、居中、三分之一屏幕等位置。你可以用快捷键，也可以把窗口拖到屏幕边缘来吸附。

Rectangle Pro 是 Rectangle 的闭源增强版，官方文档里说它是 Rectangle 的 superset，而且是单独下载的 App。我的理解是：免费版负责把窗口放到常见位置，Pro 版负责把窗口管理这件事做得更顺手、更细、更适合多屏工作。

| 功能 | Rectangle 免费版 | Rectangle Pro |
| --- | --- | --- |
| 收费方式 | 免费、开源 | 一次性买断，当前官网显示可试用 10 天 |
| 基础分屏 | 支持 | 支持，内置动作更多 |
| 拖拽吸附 | 支持屏幕边缘和角落 | 支持自定义 `Snap Areas`、`Snap Targets`、`Snap Panel` |
| 快捷键 | 常用窗口动作 | 更多内置快捷键，还能配置重复按键行为 |
| 自定义尺寸 | 支持有限的额外快捷键 | `Custom Size & Position` 可以按百分比或像素配置 |
| 鼠标/触控板操作 | 基础拖拽吸附 | `Window Throw`、`Quick Throw` 更适合少记快捷键 |
| 布局 | 手动摆放为主 | `Layouts` 可以按规则重新排布匹配到的窗口 |
| 临时隐藏窗口 | 手动最小化或切换 | `Stash` 可以把窗口藏到屏幕边缘 |
| 固定侧边窗口 | 需要手动调整 | `Pin Mode` 可以把一个 App 固定在侧边，其他窗口自动避让 |
| 同步配置 | 可导入导出配置 | 支持 iCloud 同步，也支持导入导出配置 |
| 许可证 | 免费使用 | 一个 license 可同时激活 3 台设备 |

我的建议很简单：**只用左右分屏和最大化，免费版就够了；已经买了 Pro，就优先用好 `Window Throw`、`Custom Size & Position` 和 `Layouts`。**

## 安装和基础设置

下载地址在官网：[Rectangle Pro](https://rectangleapp.com/pro/)。

安装后第一次启动，macOS 会要求给它辅助功能权限。路径一般是：

`System Settings` -> `Privacy & Security` -> `Accessibility`

找到 `Rectangle Pro`，打开开关。窗口管理软件需要这个权限来控制其他 App 的窗口位置，Apple 官方文档也把这类权限放在 Accessibility 里说明。

如果之前已经安装过免费版 Rectangle，我建议做三件事：

1. 退出免费版 Rectangle，避免两个窗口管理软件抢同一组快捷键。
2. 在 Pro 里导入或重新设置自己熟悉的快捷键。
3. 先只保留 5-8 个高频快捷键，用一周之后再加。

我自己会保留这些动作：

- `Left Half`：左半屏
- `Right Half`：右半屏
- `Maximize`：最大化
- `Center`：居中
- `Next Display` / `Previous Display`：多屏之间移动窗口
- `Restore`：恢复窗口之前的大小

窗口管理工具的关键是肌肉记忆。快捷键太多，最后反而一个都记不住。

## 最值得先学的功能

### Window Throw

`Window Throw` 是 Rectangle Pro 最有代表性的功能。官方默认触发键是 `control + command`。一般安装后已经可以用，确认路径是 `Settings` -> `Cursor Movement` -> `Window Throw` -> `Modifier Keys`。

这里默认是 `control + command`。如果 `Modifier Keys` 为空，`Window Throw` 处于停用状态。你也可以把触发方式改成鼠标按钮或触控板手势。

使用方式：

1. 鼠标放到目标窗口上。
2. 按住 `control + command`。
3. 移动鼠标，屏幕上会出现一个选择区域。
4. 松开按键，窗口会移动到对应位置。

这个功能适合快捷键记得少的人。只要记一个触发方式，然后靠鼠标方向来选择窗口位置。

我建议先把它当成「万能窗口移动器」来用：

- 往左：左半屏
- 往右：右半屏
- 往上：最大化
- 往下：居中或恢复
- 往左上/右上：对应角落

如果你有外接显示器，`Window Throw` 还可以把窗口扔到另一块屏幕上。多屏办公的人会更明显感受到这个功能的价值。

### Custom Size & Position

`Custom Size & Position` 用来创建自己的窗口尺寸和位置。官方文档说它可以按屏幕百分比配置，也可以按像素配置，还可以绑定快捷键、加入 `Window Throw`、作为拖拽吸附目标。

这里最实用的做法是先创建几个自己真正会用的尺寸：

| 名称 | 用途 | 建议参数 |
| --- | --- | --- |
| `Browser Large` | 浏览器主窗口 | 宽度 70%，高度 100%，靠左 |
| `Chat Right` | 微信、Telegram、AI 聊天 | 宽度 30%，高度 100%，靠右 |
| `Editor Center` | 写作或代码窗口 | 宽度 80%，高度 90%，居中 |
| `Preview Small` | 预览窗口 | 宽度 40%，高度 70%，靠右下 |

配置方式：

1. 打开 `Settings`。
2. 进入 `Custom Size & Position`。
3. 点击右上角 `+`。
4. 填 `Name`。
5. 设置 `Position` 和 `Size`。
6. 需要快捷键就填 `Keyboard Shortcut`。
7. 点 `View Footprint` 看一下实际位置。

这里有个小技巧：数值小于等于 `1` 的时候，可以理解成屏幕比例，比如 `0.7` 就是 70%；数值大于 `1` 的时候，可以理解成像素。

### Layouts

`Layouts` 适合每天都打开固定工作区的人。比如：

- 左边浏览器，右边编辑器
- 左边 Cursor，右边预览窗口
- 主屏写代码，副屏放终端和浏览器
- 主屏看行情，副屏放交易软件和笔记

我觉得这个功能最适合多屏用户。你可以把一组 App 的窗口位置保存起来，下次按匹配规则重新排好。

这里有个边界要搞清楚：`Layouts` 的核心是排窗口。打开 `Bring windows to front` 后，它会把匹配到的窗口提到前台；打开 `Launch closed/minimized apps` 后，它会尝试启动关闭或最小化的 App。具体网页、文档、聊天会话能否回来，取决于 App 自己的窗口恢复能力。

配置方式：

1. 先手动把几个窗口摆到你想要的位置。
2. 打开 `Settings` -> `Layouts`。
3. 点击 `+ Layout`。
4. 选择当前显示器或所有显示器。
5. 给这个布局起一个名字，比如 `Writing`、`Coding`、`Trading`。
6. 给它设置一个快捷键。

保存之后，窗口乱了直接执行这个 Layout。能匹配到的窗口会重新摆回去，比一个窗口一个窗口调整省事很多。

### Stash

`Stash` 可以把窗口藏到屏幕边缘，鼠标碰到边缘时再滑出来。它适合那些你会反复看一眼，又希望主屏保持清爽的窗口。

适合放进 Stash 的窗口：

- 微信
- Telegram
- 音乐播放器
- 下载工具
- AI 聊天窗口

我建议先给 `Stash Right` 设置一个快捷键。把聊天软件往右边一藏，需要时鼠标碰一下右边缘就出来，处理完继续藏起来。

### Pin Mode

`Pin Mode` 可以把一个 App 固定在屏幕侧边，其他窗口动作会在剩余空间里重新计算。官方文档里提到它来自 Rectangle 的 Todo Mode。

这个功能适合把待办、聊天、笔记、行情软件固定在一侧。比如把备忘录固定在右侧 30%，其他窗口只占左侧 70%。

我的建议是：**Pin 一个窗口的时候，只 Pin 信息密度高、需要常看的 App。**聊天窗口、待办、行情、AI 对话都适合。网页和编辑器通常更适合做主窗口。

## 我的推荐配置

刚开始用 Rectangle Pro，我建议按这个顺序来：

### 第一步：先把免费版体验找回来

先配置这些基础动作：

- `Left Half`
- `Right Half`
- `Maximize`
- `Center`
- `Restore`
- `Next Display`
- `Previous Display`

这一步的目标是让它先替代免费版 Rectangle。

### 第二步：开启 Window Throw

默认 `control + command` 已经可以用。你先用几天，看看手感。如果和其他软件冲突，再改成别的组合键。

我个人更喜欢把 `Window Throw` 留给鼠标操作，把普通快捷键留给键盘操作。这样脑子里分类很清楚。

### 第三步：创建 3 个自定义尺寸

先创建这 3 个：

- `Main 70 Left`：主窗口，占左侧 70%
- `Side 30 Right`：辅助窗口，占右侧 30%
- `Center 80`：居中窗口，占屏幕 80%

用一周之后，再决定是否增加更多尺寸。

### 第四步：保存 1 个 Layout

先保存一个最常用的工作区。比如写文章：

- 浏览器左侧 60%
- 编辑器右侧 40%
- 聊天软件 Stash 到右边

一开始先做 1 个 Layout。太多布局会变成新的维护成本。

这个 Layout 里建议打开两个行为：

- `Bring windows to front`：把匹配到的窗口提到前台。
- `Launch closed/minimized apps`：尝试启动关闭或最小化的 App。

这里的重点是「尝试启动 App + 重新排窗口」。具体窗口内容能否恢复，看每个 App 自己的恢复机制。

### 第五步：打开 iCloud 同步或导出配置

如果你有多台 Mac，可以在 `General` 里打开 `Sync configuration over iCloud`。如果只用一台 Mac，也建议定期 `Export Config` 备份一下。

买断软件最怕换电脑之后重新配置一遍。窗口管理工具的配置，一旦形成肌肉记忆，丢了很烦。

## 常见问题

### 快捷键没反应

优先检查两个地方：

1. `System Settings` -> `Privacy & Security` -> `Accessibility` 里是否开启 Rectangle Pro。
2. 当前快捷键是否被系统或其他 App 占用。

如果免费版 Rectangle 还在运行，先退出它。

### 拖拽吸附没反应

到 `Snap Areas` 里确认 `Snap windows by dragging` 是否开启。

如果拖窗口到屏幕边缘时没有出现半透明的 footprint，通常是权限或功能开关的问题。

### Window Throw 没反应

到 `Settings` -> `Cursor Movement` -> `Window Throw` 里检查 `Modifier Keys`。默认是 `control + command`，这里有按键组合才会触发。

如果你习惯鼠标，可以在同一页把触发方式改成鼠标按钮；如果习惯触控板，也可以设置 trackpad gesture。

### 不知道该记哪些快捷键

只记 5 个：

- 左半屏
- 右半屏
- 最大化
- 居中
- 恢复

剩下的交给 `Window Throw` 和 `Layouts`。窗口管理软件的价值是省脑子，快捷键表可以放在后面慢慢补。

### 许可证能用几台电脑？

官方 FAQ 写得很清楚：一个 license 可以同时激活 3 台设备。换电脑之前，可以在 `General` 里先移除旧设备的激活；已经拿不到旧设备，也可以在新电脑上移除最旧的激活。

### 价格值不值？

我觉得看使用频率。

每天只偶尔左右分屏，免费版 Rectangle 已经很好。每天长时间在 Mac 上工作，经常多屏、写代码、写文章、看行情、开一堆窗口，Pro 的 `Window Throw` 和 `Layouts` 能省下不少重复操作。

已经买了就别纠结了，把上面几个功能配起来，至少把钱用回来。

## 一页清单

- 权限：`Accessibility` 已经给 Rectangle Pro 打开。
- 冲突：免费版 Rectangle 已经退出。
- 快捷键：只保留 5-8 个高频动作。
- 鼠标：`Window Throw` 可以稳定触发。
- 自定义：已经创建 3 个常用窗口尺寸。
- 布局：已经保存 1 个最常用工作区。
- 备份：打开 iCloud 同步或导出配置文件。
- 复盘：用一周后删掉低频快捷键，保留真正顺手的操作。

## 总结

Rectangle Pro 最值得用的地方是减少重复摆窗口的时间。

免费版 Rectangle 适合基础分屏，Pro 版适合把窗口管理做成一套自己的工作流。先用 `Window Throw` 解决临时移动窗口，再用 `Custom Size & Position` 固定常用尺寸，最后用 `Layouts` 保存工作区。

我的最终建议是：**先配置基础快捷键 + Window Throw + 3 个自定义尺寸 + 1 个 Layout。**这四件事做好，Rectangle Pro 就已经值回大半。

## 参考链接

- [Rectangle Pro 官网](https://rectangleapp.com/pro/)：下载、试用、主要功能介绍
- [Rectangle Pro 官方文档](https://rectangleapp.com/pro/docs/)：功能说明和配置入口
- [Rectangle Pro 文档文本版](https://rectangleapp.com/pro/docs/llms.txt)：官方文档的文本版
- [Rectangle 免费版 GitHub 仓库](https://github.com/rxhanson/Rectangle)：免费版功能、安装方式和常见问题
- [Apple：Allow accessibility apps to access your Mac](https://support.apple.com/guide/mac-help/allow-accessibility-apps-to-access-your-mac-mh43185/mac)：macOS 辅助功能权限说明
