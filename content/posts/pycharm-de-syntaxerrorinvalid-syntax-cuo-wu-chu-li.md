---
title: "PyCharm 的 SyntaxError：invalid syntax 错误处理"
date: 2013-08-30T18:42:00+08:00
categories: 
draft: false
toc: true
---

首先我这个代码是没有问题的，直接用终端`python xxx.py`运行是没有问题的。 但是我下载最先版本的 PyCharm，然后导入项目，用它自带的 Run 运行后就报错`SyntaxError：invalid syntax` 然后我就去搜索，后来发现是 Python 版本的问题。我的 Linux 下有两个版本的 Python，一个是 2.7 版的一个是 3.3 版本的。安装 PyCharm 的时候调用的是 3.3 版本的。 Python2.7 打印格式是： 
    
    
    print 'a' #中间有空格
    print ('a') #也支持这种方式

Python3 不支持这种方式打印，只支持： 
    
    
    print('a') #必须有括号

**因为 print 在 python3 中已经是一个函数而不是一条命令了** 问题找到了，现在我们需要改 PyCharm 调用的 Python 版本，打开 File -> Settings -> Python In­ter­cepter -> Python In­ter­cepters ，然后在右侧点 + 添加你需要的版本即可。 现在重新运行程序，OK，正确打印，问题解决。

## Comments

**[baocaixiong](#213 "2014-12-14 00:59:00"):** 此文略无意义...

