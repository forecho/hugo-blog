---
title: "iOS开发实例（一）-“Hello World”"
date: 2012-10-21T23:01:00+08:00
categories: 
draft: false
toc: true
---

本教程是根据《iOS基础教程》一书总结而来。 1、首先打开Xcode，然后创建一个项目，选择iOS-》Application-》Single View Application（一个最简单的模板） 点击Next，如下图所示： ![](http://m3.img.libdd.com/farm5/2012/1021/21/B52ABEDA9EFB595926EEA2C69A2FFB8117E8A226476BB_728_491.JPEG) 2、然后会出现下图所示内容，你可以安装下图填好相应的资料。 ![](http://m1.img.libdd.com/farm5/2012/1021/21/F94AFF71CA5151176827687BFE2D87E2E2381CE00FDD3_728_491.PNG) 简单的解释一下： 第一项：项目名。 第二项：组织名称。 第三项：公司标识。 Class Prefix：创建所有类的前缀。 Devices：指所适用的设备。 说下最后三个复选框。 第一个就是我们说的新特性之一，用storyboards管理布局文件。 第二个ARC（自动引用计数）机制。 第三个自动生成一个测试用例。 然后点Next。   3、然后我们得到如下结构的文件： ![](http://m3.img.libdd.com/farm4/2012/1021/22/7EAAA7A485B1917F38572648AED62CF7D7D2587207020_384_339.JPEG) 简单的说一下上图文件夹： Hello World：以项目名命名的文件夹，包含了大部分代码以及组成应用程序用户界面的文件 其中BIDViewController.xib：该文件包含特定于项目主试图控制器的用户界面元素。 Supporting Files：包含项目中所需的非Objective-C类的源代码文件和资源文件。（适用这个文件夹次数比较少？）。 Frameworks：一种特殊的类库。包含代码、图像、声音文件等资源。   4、点击BIDViewController.xib文件，然后出现的就是Interface Builder（整合到Xcode里面的界面设计工具），如下图： ![](http://m2.img.libdd.com/farm5/2012/1021/22/BFD4AC2F7FEB7F3CBF008E8438EC309BEC1658C8337B7_800_500.jpg)   5、然后你就可以在右下角的类库里面直接拖过来使用。找到Label，直接拖到用户界面任意位置，然后该文字为“Hello World”，OK，这时候基本上算是完成了，点击一下Xcode左上角的Run，然后自动会调用模拟器，然后界面会出现如下所示界面，代表你成功了。 ![](http://m2.img.libdd.com/farm4/2012/1021/22/63FED48E278A6DA9522D044617BA3651B8C4AA4240180_368_716.PNG)   6、回过头来，我们还可以在标签属性里面直接更改资料，然后保存，然后Run。   第一节完毕，整个过程我们还没有动过一个代码。（注意，网上有说对于新手来说最好不要用ARC来自动管理内存，因为到后面你会有很大的麻烦。但是本教程是根据《iOS基础教程》总结的，书上是这么使用的，对于新手来说的话，只能先这样了。）