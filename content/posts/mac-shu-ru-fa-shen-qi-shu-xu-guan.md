---
title: "Mac输入法神器-鼠须管？"
date: 2012-11-30T16:53:00+08:00
categories: 
draft: false
toc: true
---

是不是神器我不知道，只有等我用过一阵子之后才能知道。 本篇文章就是讲讲我安装的过程以及一些最基本的配置，还有就是体验一下刚装上的这个神器。 [官方的下载地址](http://code.google.com/p/rimeime/)在这~，但是有时候会很慢，我共享的一个地址，目前最新的版本0.9.11，你可以在这[下载~](http://pan.baidu.com/share/link?shareid=138796&uk=2684558169) 下载完之后，尼玛找半天没找到，为此还重启了一回电脑还是没发现，后来是网上搜了一些资料，这才发现位置，你可以参考下图：![](http://m1.img.libdd.com/farm5/2012/1128/21/E15D546A9674892A5BA87D1B06649D481C48B49D2E937_664_353.JPEG) 我只想说隐藏的好深，不注意看很难找到。  安装好之后简单的配置一下： **一. 配置简繁体输入** 鼠须管默认是繁体输入，需要切换到简体输入的话，请用快捷键 **control+“~”**  打开切换菜单，选“漢字→汉字” 即可。 ![](http://m3.img.libdd.com/farm4/2012/1128/21/63BCA0BC2A6649E98F6A30371DBC8FB764A5D0362C762_245_270.PNG) **二、配置显示方式** 复制default.yaml和squirrel.yaml，并重命名为default.custom.yaml和squirrel.custom.yaml。 1、default.custom.yaml可以改输入的候选词个数。代码如下： 
    
    
    patch:
      "menu/page_size": 8

2、squirrel.custom.yaml可以改输入法[配色方案](https://gist.github.com/2309739)和字号、横向展示等功能，具体请参考这个[网址](http://code.google.com/p/rimeime/wiki/CustomizationGuide)。代码如下： 
    
    
    patch:
      us_keyboard_layout: true      # 鍵盤選項：應用美式鍵盤佈局
      style/horizontal: true        # 候選窗横向顯示
      style/font_face: "Hiragino Sans GB W3"    # 我喜歡的字體名稱
      style/font_point: 18          # 字號
      style/corner_radius: 5       # 窗口圓角半徑
      style/border_height: 8        # 窗口邊界高度，大於圓角半徑才有效果
      style/border_width: 8         # 窗口邊界寬度，大於圓角半徑才有效果
      style/color_scheme: luna      # 選擇配色方案