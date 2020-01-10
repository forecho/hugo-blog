---
title: "Mac Sublime Text 实现 Chrome 快速预览"
date: 2015-01-08T22:02:00+08:00
tags: ["编程工具"] 
draft: false
toc: true
---
## 安装

按照 Tools -> Build System -> New Build System... 新建一个「Build System」文件。
写入以下代码：

```
{
    "cmd": ["open", "-a", "/Applications/Google Chrome.app", "$file"]
}
```

保存在「~/Library/Application Support/Sublime Text 2/Packages/User」，文件名为「Chrome.sublime-build」。

##使用方法

在 Tools -> Build System 选择 Chrome，然后直接按「command+b」快捷键就自动打开 Chrome 了。

##参考链接
1. [Sublime Text 2 Browser Preview(youtube 视频要翻墙)](https://www.youtube.com/watch?v=u8ufaPD-AnQ)
2. [Set up Sublime Text to Preview Your Code in a Web Browser](http://www.granneman.com/webdev/editors/sublime-text/set-up-sublime-text-to-preview-your-code-in-a-web-browser/)
3. [Sublime Text 2 keyboard shortcut to open file in specified browser (e.g. Chrome)](http://stackoverflow.com/questions/8023879/sublime-text-2-keyboard-shortcut-to-open-file-in-specified-browser-e-g-chrome)