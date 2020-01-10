---
title: "Mac 利器 Hammerspoon 使用指南"
date: 2019-12-26T21:48:00+08:00
tags: ["mac"] 
draft: false
toc: true
---

## 引言

今天要给大家分享的是一款 Mac 利器 Hammerspoon，通过编写 Lua 脚本可以让这款软件实现以下功能

- 管理窗口大小以及位置
- 软件启动器
- 给特定的软件设置中/英文输入法

你甚至可以通过编写代码实现

- 休眠状态关闭蓝牙功能
- 连上公司 Wi-Fi 自动静音
- ……

接下来详细讲解如何实现以上功能。不会 Lua？没关系，只要你有一点点编程经验，代码看起来都不是问题。

<!--more-->

## 实现

我自己的配置代码已经在 GitHub 上开源，有兴趣的可以直接去读源码 - [forecho/hammerspoon-config
](https://github.com/forecho/hammerspoon-config)

### 目录结构

```
.
├── README.md
├── config.lua
├── init.lua
└── modules
    ├── bluetoothSleep.lua
    ├── defaultInput.lua
    ├── input.lua
    ├── launcher.lua
    ├── reload.lua
    └── window.lua
```

- `init.lua` 是入口文件
- `config.lua` 是配置文件，通过修改此文件，定制化自己的需求
- `modules` 是每个模块的文件夹，具体功能都在这里实现

### 初始化文件

`init.lua`

```lua

require "modules.reload"
require "config"
require "modules.window"
require "modules.launcher"
require "modules.input"
require "modules.defaultInput"
require "modules.bluetoothSleep"

-- 调试代码
hs.hotkey.bind({'cmd', 'shift'}, 'h', function() 
	hs.alert('Hello World') 
	speaker = hs.speech.new()
	speaker:speak("Hammerspoon is online")
	hs.notify.new({title="Hammerspoon launch", informativeText="Boss, at your service"}):send()
end)

```

### 配置文件

`config.lua`

```lua

windowHotkey = {'control','command'}
launcherHotkey = {'option'}
inputHotkey = {'option'}

applist = {
    {shortcut = 'Q',appname = 'QQ'},
    {shortcut = 'G',appname = 'Google Chrome'},
    {shortcut = 'C',appname = 'Visual Studio Code'},
    {shortcut = 'I',appname = 'iTerm'},
    {shortcut = 'P',appname = 'PHPStorm'},
    {shortcut = 'W',appname = 'WeChat'},
    {shortcut = 'O',appname = 'Postman'},
    {shortcut = 'Y',appname = '企业微信'},
}

appInputMethod = {
    {'/Applications/iTerm.app', 'English'},
    {'/Applications/Visual Studio Code.app', 'English'},
    {'/Applications/PHPStorm.app', 'English'},
    {'/Applications/Xcode.app', 'English'},
    {'/Applications/Google Chrome.app', 'English'},
    {'/System/Library/CoreServices/Finder.app', 'English'},
    {'/Applications/DingTalk.app', 'Chinese'},
    {'/Applications/Kindle.app', 'English'},
    {'/Applications/NeteaseMusic.app', 'Chinese'},
    {'/Applications/WeChat.app', 'Chinese'},
    {'/Applications/System Preferences.app', 'English'},
    {'/Applications/Dash.app', 'English'},
    {'/Applications/MindNode.app', 'Chinese'},
    {'/Applications/QQ.app', 'Chinese'},
    {'/Applications/企业微信.app', 'Chinese'},
    {'/Applications/wechatwebdevtools.app', 'English'},
    {'/Applications/Sketch.app', 'English'},
}

```

### 实现自动输入法切换功能

`defaultInput.lua`

```lua

local function Chinese()
    hs.keycodes.currentSourceID("com.sogou.inputmethod.sogou.pinyin")
end

local function English()
    hs.keycodes.currentSourceID("com.apple.keylayout.ABC")
end

function updateFocusAppInputMethod()
    local focusAppPath = hs.window.frontmostWindow():application():path()
    for index, app in pairs(appInputMethod) do
        local appPath = app[1]
        local expectedIme = app[2]

        if focusAppPath == appPath then
            if expectedIme == 'English' then
                English()
            else
                Chinese()
            end
            break
        end
    end
end

-- helper hotkey to figure out the app path and name of current focused window
hs.hotkey.bind({'ctrl', 'cmd'}, ".", function()
    hs.alert.show("App path:        "
    ..hs.window.focusedWindow():application():path()
    .."\n"
    .."App name:      "
    ..hs.window.focusedWindow():application():name()
    .."\n"
    .."IM source id:  "
    ..hs.keycodes.currentSourceID())
end)

-- Handle cursor focus and application's screen manage.
function applicationWatcher(appName, eventType, appObject)
    if (eventType == hs.application.watcher.activated) then
        updateFocusAppInputMethod()
    end
end

appWatcher = hs.application.watcher.new(applicationWatcher)
appWatcher:start()

```

### 切换输入法

`input.lua` 实现切换输入法功能，这个已经弃用了，最后还是使用系统自带的快捷键了，代码放出来给大家参考：

```lua

local function Chinese()
    hs.keycodes.currentSourceID("com.sogou.inputmethod.sogou.pinyin")
end

local function English()
    hs.keycodes.currentSourceID("com.apple.keylayout.ABC")
end


hs.hotkey.bind(inputHotkey, 'S', function() 
    Chinese()
end)

hs.hotkey.bind(inputHotkey, 'E', function() 
    English()
end)

local function cycleInputMethod()
    if hs.keycodes.currentSourceID() == "com.apple.keylayout.ABC" then
        hs.keycodes.currentSourceID("com.sogou.inputmethod.sogou.pinyin" )
    elseif hs.keycodes.currentSourceID() == "com.sogou.inputmethod.sogou.pinyin" then
        hs.keycodes.currentSourceID("com.apple.inputmethod.SCIM.ITABC")
    elseif hs.keycodes.currentSourceID() == "com.apple.inputmethod.SCIM.ITABC" then
        hs.keycodes.currentSourceID("com.apple.keylayout.ABC")
    end
end

hs.hotkey.bind(inputHotkey, '.', cycleInputMethod)

```

### App 启动

`launcher.lua`，可以实现特定的快捷键启动指定的软件，使用最频繁的功能了，妥妥的提升效率。实现代码如下：

```lua

hs.fnutils.each(applist, function(entry)
    hs.hotkey.bind(launcherHotkey, entry.shortcut, entry.appname, function()
        hs.application.launchOrFocus(entry.appname)
    end)
end)

```

修改 `config.lua` 文件，定制化自己的需求。

### 自动重载配置

不用手动点击刷新，自动重载 Hammerspoon 配置

```lua

local function reloadConfig(paths)
    doReload = false
    for _,file in pairs(paths) do
        if file:sub(-4) == ".lua" then
            print("A lua config file changed, reload")
            doReload = true
        end
    end
    if not doReload then
        print("No lua file changed, skipping reload")
        return
    end

    hs.reload()
end

configFileWatcher = hs.pathwatcher.new(os.getenv("HOME") .. "/.hammerspoon/", reloadConfig)
configFileWatcher:start()

```


### 窗口管理

`window.lua`，我只有最大化和两个显示器换屏的需求，所以只实现的这个功能，还可以实现二分之一屏幕的需求，参考本文末尾给的参考链接。

```lua

hs.hotkey.bind(windowHotkey, 'return', function()
    hs.grid.maximizeWindow()
end)

hs.hotkey.bind(windowHotkey, 'F', function() 
    hs.window.focusedWindow():toggleFullScreen()
end)

hs.hotkey.bind(windowHotkey, 'left', function()
    local w = hs.window.focusedWindow()
    if not w then
        return
    end
    local s = w:screen():toWest()
    if s then
        w:moveToScreen(s)
    end
end)

hs.hotkey.bind(windowHotkey, 'right', function()
    local w = hs.window.focusedWindow()
    if not w then
        return
    end
    local s = w:screen():toEast()
    if s then
        w:moveToScreen(s)
    end
end)

```

### 休眠自动关闭蓝牙

`bluetoothSleep.lua`，无意中发现 Mac 电脑休眠的时候蓝牙居然还出于开着的状态，感觉有点浪费电，使用写了一个脚本实现蓝牙自动开关功能。

使用这个功能之前先要安装 `blueutil`

```sh
brew install blueutil
```

```lua

function bluetoothSwitch(state)
    -- state: 0(off), 1(on)
    cmd = "/usr/local/bin/blueutil --power "..(state)
    result = hs.osascript.applescript(string.format('do shell script "%s"', cmd))
end

function caffeinateCallback(eventType)
    if (eventType == hs.caffeinate.watcher.screensDidSleep) then
      print("screensDidSleep")
    elseif (eventType == hs.caffeinate.watcher.screensDidWake) then
      print("screensDidWake")
    elseif (eventType == hs.caffeinate.watcher.screensDidLock) then
      print("screensDidLock")
      bluetoothSwitch(0)
    elseif (eventType == hs.caffeinate.watcher.screensDidUnlock) then
      print("screensDidUnlock")
      bluetoothSwitch(1)
    end
end

caffeinateWatcher = hs.caffeinate.watcher.new(caffeinateCallback)
caffeinateWatcher:start()

```

## 最后

Hammerspoon 能做的事情当然远不知这些，但是这些功能已经让我事半功倍了，最重要的是这个软件开源免费！

## 参考链接

- [Hammerspoon](https://www.hammerspoon.org/)
- [推荐一个 MacOS 上用了就无法自拔的神器 [Hammerspoon] 和我的配置方案](https://thinkhard.tech/2019/04/08/hammerspoon-introduce/)
- [『Hammerspoon』Mac 锁屏自动开关蓝牙](https://zhuanlan.zhihu.com/p/59737941)
