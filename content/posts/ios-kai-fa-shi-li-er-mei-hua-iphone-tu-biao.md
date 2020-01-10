---
title: "iOS开发实例（二）-“美化iPhone图标”"
date: 2012-10-22T23:53:00+08:00
categories: 
draft: false
toc: true
---

1、iPhone图标一般都是.png格式文件。 2、一般都是57px*57px，一般命名为icon.png；114px*114px为Retina屏专用的，一般命名为icon@2x.png。 3、创建图标的时候为一般正方形图片即可，iPhone将自动为图标调整圆角边缘，并且自动加上玻璃质感的效果。   图标制作好之后，按照下面1-》2->3找到相应的位置： ![](http://m1.img.libdd.com/farm5/2012/1022/23/81BA7EC20DDB67C06793B4602D536F409E313CB6052A9_800_524.jpg) 把之前制作的.png文件拖到相应的位置即可（114px的文件放在右边窗口）。 然后两个图标文件会自动载入，为了方便起见我们可以把这两个文件拖到Supporting Files文件夹里。 我们可以在Hello_World_Info.plist能找到相应的配置，如下图： ![](http://m1.img.libdd.com/farm5/2012/1022/23/B8658349A69C43F4D5277505CED658B93A011192B8E30_800_383.jpg)   PS:Bundle identifier（束标示符）标准命名规则是：顶级Internet域名，之后是点号，之后是公司名或者是组织名，再点号，最后是应用名。如果要改束标示符的话就在这里改。 未完待续————