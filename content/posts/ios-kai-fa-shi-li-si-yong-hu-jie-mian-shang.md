---
title: "iOS开发实例（四）- 用户界面（上）"
date: 2012-11-18T22:24:00+08:00
categories: 
draft: false
toc: true
---

这次要完成的效果图如下： ![](http://m3.img.libdd.com/farm5/2012/1117/22/A54C73690D46FFAFDE62B89335284BDAAB8B30EC1B215_366_716.PNG)![](http://m2.img.libdd.com/farm5/2012/1117/22/14C06C68CF4FB1D4A96DFA310C6D45AACD078888FB659_361_709.PNG)![](http://m1.img.libdd.com/farm4/2012/1117/22/170D6C85A1976A9E739FB616BF3CDC2FF9B1C00C01240_361_709.PNG)![](http://m3.img.libdd.com/farm4/2012/1117/22/B067D08869DD06595F1C2A9FF2557B312E3224496905F_361_709.PNG) **一、创建一个名为`Control Fun`的项目**，选择的依旧的`Single View Application`模板。  **二、用Photoshop做一张宽度小于300px高度小于100px的图片**，此处我们使用的是（172px*80px），下图文件： ![](http://m3.img.libdd.com/farm4/2012/1117/23/8AAA7EA45E95ADE646F32F8C5DBEBF38D0E650BC3F1CA_172_80.PNG) 然后把图片直接拖到我们的Xcode项目中。记得在提示框中选中`Copy items into destination group's folder(if needed)`复选框。PS:我习惯性的把图片文件拖到Supporting Files文件夹下面。   **三、根据上一节学到的知识**，我们在BIDViewController.xib中制作出下图的效果来： ![](http://m2.img.libdd.com/farm4/2012/1118/00/1665242F22F864799674D3BCDC06F4FEFF57D2EC28608_310_189.PNG) 1、在IB中找到`image View`库，然后拖到视图中，刚拖过来时会自动全屏显示，然后需要我们手动调试大小，大致调的差不多就好了，然后在右边属性框找到`Image View`中，下拉选择我们需要的图片文件。（此处系统会自动识别图片文件）。 2、调整图片大小：有个最简单的方法，依次选择Editor->Size to Fit Content，那么Xcode会自动调试成图片原尺寸大小。 3、调整图片的位置：最简单的方法依次选择Editor->Align->Horizontal Conter in Container，Xcode会自动帮我们把图片放在视图中间位置。 4、接下来就是该设置图片属性了，下面是我总结的一张表（其实是`Text`文本库的属性，但是后面的属性跟图片是一样的）： ![](http://m3.img.libdd.com/farm4/2012/1117/23/6615B31A2F2ECC10456802FE2FF233B5969E1595FA756_600_950.PNG) 此处基本上所以属性我们都不用该，只需要取消`Drawing `下面的`Clip Subviews`和`Autoresize Subviews`这两个选项即可，因为此处我们根本不需要这两项。 5、在IB里面直接拖过来各俩个Text和Lable库，然后按照上上图布局即可。（小窍门：同时选中Lable的两个标签然后依次选择Editor->Align->Right Edges可以使两个标签文字右对齐）。 6、选中Text标签，然后根据自己的需要来选择或修改属性。（可以查看上图注释）此处我们在`Placeholder`添加了Type in a name；`Capitalization` 选择Words；`Drawing`选中Opaque取消Clears Graphics Context和Clip Subviews；Number的Text基本上和Name的Text一样，除了`Placeholder`添加了Type in a number；`Capitalization`保持默认，`Keyboard`选择Number Pad。   **四、创建链接和输出口:** 根据上一节学习的，按住`control`按键拖动`Text`到.h文件中，Name的Text在Name字段中填写`nameField`，Number的Text在Name字段中填写`numberField`，如下图： ![](http://m2.img.libdd.com/farm4/2012/1118/00/5444F5C95288E8863F0DF72BE724104CB06AB8D43ED25_285_151.PNG) 此时我们可以Run一下我们的程序，得到如下结果即为成功： ![](http://m3.img.libdd.com/farm5/2012/1118/00/7D5852BC22C9F050865DAF1A208232F66B035BA0A1FFC_365_708.PNG)   **五、关闭键盘** 1、完成输入之后关闭键盘：只需在.h文件@end前加入如下代码即可： 
    
    
    - (IBAction)textFieldDoneEditing:(id)sender;

在.m文件中@end前加入如下代码： 
    
    
    - (IBAction)textFieldDoneEditing:(id)sender {
        [sender resignFirstResponder];
    }

选中Name的Text，找到右边属性的最后一个图标，找到`Did End ON Exit` 将旁边那个圈拖到`File's Owner`，如下图，然后选择`textFieldDoneEditing`操作。 ![](http://m3.img.libdd.com/farm5/2012/1118/00/05C715A190F0D622362E024A4CCEE36B0176BA992ABF4_800_74.jpg) 2、通过触摸背景关闭键盘:在.h文件@end前添加如下代码： 
    
    
    - (IBAction)backgroundTop:(id)sender;

在.m文件中@endq前加入如下代码： 
    
    
    -(IBAction)backgroundTop:(id)sender{
        [nameField resignFirstResponder];
        [numberField resignFirstResponder];
    }

现在我们需要把View在右边属性中找到class从`UIView`改成`UIControl`，只有这样背景才能有触发操作。然后找到右边属性的最后一个图标，找到`Tounch Down` 将旁边那个圈拖到`File's Owner`，选择`backgroundTop`操作。   接下来我们Run一下，如果程序没有出问题的话，就能实现点击背景关闭键盘功能。   \-----------上部分完，下部分请点击这里。