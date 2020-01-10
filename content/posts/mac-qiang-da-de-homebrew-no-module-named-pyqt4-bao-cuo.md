---
title: "Mac强大的Homebrew - No module named PyQt4报错"
date: 2013-09-01T01:18:00+08:00
categories: 
draft: false
toc: true
---

安装了这个东西之后可以帮助你非常的省事的安装其他应用程序。[官网链接。](http://brew.sh/) 其实还有一个MacPorts，跟这个Homebrew类似的功能，但是我更喜欢Homebrew的简洁。（这两个貌似不兼容，最好不要同时安装。） 一条`brew install pyqt`命令就可以安装完PyQt4的插件了。我就是用`brew install mongondb`安装的mongonDB。 这样安装完PyQt之后然后简单写了第一个程序，代码如下： 
    
    
    import sys
    from PyQt4 import QtGui
    
    app = QtGui.QApplication(sys.argv)
    
    widget = QtGui.QWidget()
    widget.resize(250, 150)
    widget.setWindowTitle('simple')
    widget.show()
    
    sys.exit(app.exec_())

运行的时候报错：No module named PyQt4 但是明明我已经安装好了，通过搜索发现是PATH路径的问题。 解决方法是：vim .bash_profile 打开这个文件，然后添加下面代码： 
    
    
    export PATH=/usr/local/bin:$PATH
    export PATH=/usr/local/share/python:$PATH

OK，再运行前面的程序，正常弹出一个窗口，说明你成功了。 如果不确定自己的Python路径，可以用`type python`命令查看。   参考链接： [https://github.com/mxcl/homebrew/issues/6176 ](https://github.com/mxcl/homebrew/issues/6176 ) <https://github.com/mxcl/homebrew/issues/10324>