---
title: "iOS开发实例（四）- 用户界面（中）"
date: 2012-11-20T08:42:00+08:00
categories: 
draft: false
toc: true
---

接着[上一篇文章](/archives/739)。这一篇文章主要讲滑块功能。 **一、添加滑块和标签，达到下图效果：** **   ![](http://m3.img.libdd.com/farm5/2012/1119/21/22D01842EE388B6C934B9DDF0218F10C1FF33EAB6C3FB_317_47.PNG) ** 1、打开.xib文件直接在IB库里面拖过来`Slider`和`Label`控件，然后改动位置以及长度。** ** 2、改动Slider属性参数：Value的`Maximum`改为100（即最大值为100），Current改为50（即默认值为50），如下图： ![](http://m2.img.libdd.com/farm5/2012/1119/21/A1DE08CFA129B877F21C5CE05D756F2DFB041729A42C3_243_94.PNG) 3、改Label的Text为100，然后依次选择Editor->Size to Fit Content，这样滑块大小正好包容“100”的字符长度。  **二、实现滑块功能：** 1、按住control拖动Slider控件到.h文件，创建一个操作：将Connection改为`Action`，name输入`sliderChanged`，然后回车，如下图： ![](http://m2.img.libdd.com/farm5/2012/1119/21/A05C6551F9146869B37805445182F63FBBC0FD9B0D0BC_284_170.PNG) 2、按住control拖动Lable控件到.h文件，创建一个输出口：在name处输入`sliderLable`，然后回车，如下图： ![](http://m2.img.libdd.com/farm4/2012/1119/21/1C55F373F18FE8FC03228FFF9765B5C1633FD7F6853A7_281_154.PNG) 3、在.m文件找到空的`sliderChanged`方法，添加代码，变成如下代码： 
    
    
    - (IBAction)sliderChanged:(id)sender {
        UISlider *slider = (UISlider *)sender;//将sender赋给一个UISlider指针
        int progressAsInt = (int)roundf(slider.value);//获取滑块的当前值，四舍五入为整数。然后赋值给整型变量
        sliderLabel.text = [NSString stringWithFormat:@"%d",progressAsInt];//创建一个字符串，使其包含该数值，并把字符串赋给标签
    }

现在我们Run一下，如果你滑动滑块，左边的数字也在变化，那就证明你成功了。