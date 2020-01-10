---
title: "安装 Python pip"
date: 2014-11-18T20:05:00+08:00
tags: ["Python"] 
draft: false
toc: true
---
## Windows 安装

1. 先下载 [get-pip.py](https://raw.github.com/pypa/pip/master/contrib/get-pip.py)
2. 再运行`python get-pip.py`执行安装程序
3. 添加环境变量到计算机，例如：`C:\Python27\Scripts`

##Ubuntu安装
1. 在终端运行`sudo apt-get install python-pip`

##解决asciii码错误
<!--more-->
这个问题是由环境默认编码导致的。

**Windows的解决办法是：**
在 `C:Python27\Lib\site-packages` 建一个文件 sitecustomize.py，输入以下内容，Python 会自动运行这个文件。

```python
import sys
sys.setdefaultencoding('gb2312')
```
**Ubuntu的解决办法是**
在终端输入以下命令：

```sh
echo $LANG
export LANG=zh_CN.GBK
echo $LANG
```

## 参考链接

1. [How to install pip on Windows?](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)
2. [请教一个pip install 出现报asciii码错误的问题。](http://www.v2ex.com/t/90659)