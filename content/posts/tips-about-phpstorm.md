---
title: "关于 PhpStorm 的小技巧"
date: 2016-01-29T16:36:00+08:00
tags: ["PhpStorm", "工具", "效率"] 
draft: false
toc: true
---

## 前言

如果一个工具是你每天工作都必须要用的，请认真阅读官方文档，花几个小时好好研究使用技巧，这将为你以后工作带来很高的效率。

这篇文章内容其实很早就在我印象笔记里面了，今天整理出来，分享给大家。

## 快捷键

**常用快捷键**

* 全局搜索文件  按两下shift
* 显示项目类列表 alt + 1
* 显示方法列表  alt + 7
* 全局搜索类 ：ctrl+n
* 更新注释：alt+enter 

<!--more-->

**Mac 快捷键**

1. 显示或者隐藏左边栏：command+1
2. 查找文件名：command+shift+o
3. 查看文件结构：command+F12
4. 查看函数、方法：command+alt+o
5. 关闭所有文件的时候 command+↑ 、command+n 快速新建文件
6. command+e 查看历史打开文件
7. Live  Templates：先选中一个模板，再按两下 shift ，再输入 「Live」找到「Save as Live  Templates」就可以新建一个模板
8. alt+command+L 格式化代码
9. 代码块转换成独立方法：control+t
10. 函数操作 command+n / control+enter 
11. 方法参数操作：alt+enter
12. 调用其他类的时候自动use：设置-》搜索「Import」勾选「Enable auto-import in namespace scope」
13. 多个光标：按住 alt 健点击鼠标
14. 多选：control+g
15. 分屏：「Window」-》「Editor Tabs」-》Split XX
16. 设置命名空间：设置-》Directories-》Sources-》p


**Windows 快捷键**

1. 项目名右键选择"Local History -> Show History"可查看本地修改记录
2. Ctrl + E 可查看最近打开文件或项目
3. 打开File -> Setting -> Editor，选择Appearance下面的Show Method Separators。它会将你的代码按方法，用灰色线框进行智能分割。你还可以使用：alt+↑或↓，在方法之间进行跳转
4. Ctrl + Shift + V，可选择要粘贴的最近内容
5. Ctrl + D，复制粘贴选中的文本
6. Ctrl + Y，删除当前行或选中行
7. Ctrl + Alt + 左右方向键，定位到上一次编辑的位置
8. Alt + 上下方向键，跳转到上/下函数
9. Alt + 左右方向键，导航标签切换
10. Ctrl + N，根据类名称查找
11. Ctrl + Shift + N，根据文件名查找
12. Ctrl + Shift + Alt + N，根据函数名查找
13. Ctrl + Shift + F，Find in Path
14. Ctrl + Shift + I，查看变量初始化的值
15. Ctrl + F12，快速查看当前文件的所有方法
16. Ctrl + /，单行注释
17. Ctrl + Shift + /，多行注释
18. 修改默认打开的文件模版："file" ---> "setting" ---> "file and code template"
19. /** + Enter 或者  /** + 空格键，自动生成注释
20. Ctrl + Alt + L，格式化代码
21. 选中文件或者文件，Alt+Insert 新建文件

**更换为 sublime 快捷键**

因为以前用习惯了 sublime，改不过来快捷键方式了，所以但是了这个项目 [PHPStorm-Config](https://github.com/forecho/PHPStorm-Config)


## 其他

**视频教程**

<https://laracasts.com/series/how-to-be-awesome-in-phpstorm>

**换主题**

<http://daylerees.github.io/>
<https://github.com/daylerees/colour-schemes>

添加主题：
```
cd ~/Library/Preferences/WebIde80/colors
wget https://raw.githubusercontent.com/daylerees/colour-schemes/master/jetbrains/yule.icls
```

**安装插件**

- command+shift+a 输入 plugins 
- 点击 Browse Repositories
- 输入 color 选择 color ide 安装 重启IDE

**更换新建文件模板**

Setting -> File and Code Templates

```
/**
* author     : forecho <caizhenghai@gmail.com>
* createTime : ${DATE} ${TIME}
* description:
*/
```