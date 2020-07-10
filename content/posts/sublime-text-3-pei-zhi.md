---
title: "Sublime Text 3配置"
date: 2014-02-10T11:37:00+08:00
tags: ["Sublime"] 
draft: false
toc: true
---

**安装 Packages** 

1、ctrl+` 2、输入命令：

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

**Preferences > Settings -User** 基本的配置：

```
{
    "font_size": 13,
    "ignored_packages":
    [
        "Vintage"
    ],
    "spell_check": false,
    "font_face": "YaHei Consolas Hybrid",
    "trim_trailing_white_space_on_save": true, //去除空格
    "update_check": false,
    "word_wrap": false
}
```

一些基本的插件：

- Emmet：必装插件。

- FileHeader：自动给文件加文件的注释信息。

- TortoiseSVN：结合TortoiseSVN客户端使用。

- FileDiffs：文件对比插件。

- BracketHighlighter ：高亮显示匹配的括号、引号和标签

- TrailingSpaces： 高亮显示多余的空格和Tab

- jQuery：必装插件

- Alignment ：等号对齐

- [CodeFormatter](https://github.com/akalongman/sublimetext-codeformatter)：代码格式化

注：可能是QQ和这个冲突了，自行设置了一下快捷键就可以用了。 打开 `Preferences=>Package Settings=>Alignment=>Key Bindding - User` 然后写入：

```
[
    { "keys": ["ctrl+alt+f"], "command": "alignment" }
]
```

或者改成其他不冲突的快捷键即可。