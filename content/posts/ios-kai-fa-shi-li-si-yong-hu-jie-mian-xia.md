---
title: "iOS开发实例（四）- 用户界面（下）"
date: 2012-11-20T08:47:00+08:00
categories: 
draft: false
toc: true
---

**[上上篇文章**](/archives/739)****我们讲了键盘的调用以及关闭，**[上篇文章](http://old.forecho.com/archives/749)我们讲了滑块控件，本篇文章主要讲开关、按钮和分段控件。** ** ** **一、添加分段控件：**打开.xib文件在IB里找到`Segmented Control`分段控件，直接拖过来，然后调整大小，改First名为`Switches`，改Second名为`Button`，完成之后如下图：   **二、添加开关控件：** 1、在IB里直接找到`Switch`开关控件拖过来，然后按住键盘option键。拖动刚才的Switch，移动到另外一边生成另外一个开关。 2、链接开关的输出口和操作：按住control拖动左边的开关控件到.h文件的@end前，添加一个输出口，name输入`leftSwitch`，然后回车。然后右边的开关也是同样的操作，name名为`rightSwitch`，效果如下图： ![](http://m2.img.libdd.com/farm5/2012/1119/22/C06B25713F77FD53D22F4042DC3593E3D0FCC872901A9_283_150.PNG)![](http://m3.img.libdd.com/farm5/2012/1119/22/523B0610F7DCD9B09019DCE4EB4538232BFC9BB7525A3_279_152.PNG) 3、关联两个开关：这个比较关键，关联之后，操作一个开关另一个开关也跟着动。先按住control键拖动左边的开关到.h文件的@end前，然后将Connection改为`Action`，将name输入`switchChanged`，然后回车。如下图： ![](http://m1.img.libdd.com/farm4/2012/1119/22/EE2A8CCB8BAE30A666118D77A52E9E80EBB369DCF6140_277_169.PNG) 比较关键的是下一步，你要按住control键拖动右边的开关控件，**拖到已经创建的`switchChanged`上面。**只有这样才能时两个开关关联起来，并且同时操作。如下图： ![](http://m1.img.libdd.com/farm4/2012/1119/22/AF1E6523572A757EC9FF5BD2CBEC7A267F31FCB556786_493_131.PNG) 最后，按住control把分段控件拖到.h文件中，创建一个操作方法：将Connection改为`Action`，将name输入`toggleControls`。 4、实现功能还需要在.m的`switchChanged`操作方法添加代码，完成之后代码如下： 
    
    
    - (IBAction)switchChanged:(id)sender {
        UISwitch *whichSwitch = (UISwitch *)sender;
        BOOL setting = whichSwitch.isOn;//获得开关状态
        [leftSwitch setOn:setting animated:YES];//设置开关状态
        [rightSwitch setOn:setting animated:YES];//设置开关状态
    }

这时候我们Run一下程序，当你操作一个开关时另一个开关也同时动，那就说明我们成功了。   **三、添加按钮** 1、在IB库里面找到`Round Rect Button`，直接拖到.xib文件的开关控件上面，并且调整大小，使之完全覆盖两个开关控件，并且添加名`Do Something`。 2、按住control键拖到按钮到.h文件的@end前，添加一个输出口，在name输入`doSomethingButton`，然后回车。再一次按住control键拖到按钮到.h文件的@end前，添加一个操作，将Connection改为`Action`，将name输入`buttonPressed`。如下图： ![](http://m3.img.libdd.com/farm5/2012/1119/22/EEE2AE4F26B7C93FD97A543A013239A8C539ADA29C8C5_278_152.PNG)![](http://m1.img.libdd.com/farm4/2012/1119/22/EBE28CE67D9A1798C6F1217CEF778041FF8EE2E313171_275_169.PNG) 3、为了让程序启动时隐藏按钮，在按钮属性中找到View部分的Drawing，选中`Hidden`复选框。   **四、实现分段控件的操作方法** **   **在.m文件中找到`toggleControls`方法，添加操作，完成之后代码如下： 
    
    
    - (IBAction)toggleControls:(id)sender {
        //0 == switchs index 判断当前选择的分段控件是哪一部分
        if ([sender selectedSegmentIndex] == 0) {
            leftSwitch.hidden = NO;
            rightSwitch.hidden = NO;
            doSomethingButton.hidden = YES;
        }else{
            leftSwitch.hidden = YES;
            rightSwitch.hidden = YES;
            doSomethingButton.hidden = NO;
        }
    }

这时候Run一下程序，如果能使用分段控件正确切换开关和按钮的话，那么你成功了。   **五、实现操作表和警报**（两者类似但是有区别，操作表需要用户做出选择，然后根据选择会得到不同的结果，而警报只是一个通知，可能只有一个按钮） 1、遵从操作表的委托方法：在.h文件中添加`<UIActionSheetDelegate>`，位置如下： 
    
    
    @interface BIDViewController : UIViewController <UIActionSheetDelegate>

2、显示操作表：在.m文件中找到`buttonPressed`方法。添加代码之后如下： 
    
    
    - (IBAction)buttonPressed:(id)sender {
        UIActionSheet *actionSheet = [[UIActionSheet alloc] //分配一个UIActionSheet对象并进行初始化
                                      initWithTitle:@"Are you sure?" //设置标题，将会显示在Action Sheet的顶部
                                      delegate:self //设置操作表的委托，该表被按下时收到通知。通过self传递。
                                      cancelButtonTitle:@"No Way" //设置取消按钮的标题，将会显示在Action Sheet的最下边
                                      destructiveButtonTitle:@"Yes,I'm Sure!" //设置第一个确定按钮的标题，“继续”按钮
                                      otherButtonTitles:nil];
                                      //otherButtonTitles: @"New Button 1", @"New Button 2", nil]; //可以设置任意多的确定按钮
        [actionSheet showInView:self.view]; //显示操作表
    }

3、然后在下面添加这个方法，代码如下： 
    
    
    //actionSheet didDismissWithButtonIndex是UIActionSheetDelegate委托方法中的一个
    - (void)actionSheet:(UIActionSheet *)actionSheet didDismissWithButtonIndex:(NSInteger)buttonIndex{
        if (buttonIndex != [actionSheet cancelButtonIndex]) {//buttonIndex表示用户所轻触的按钮的编号，编号从上往下从0开始。actionSheet cancelButtonIndex表示取消按钮的编号。
            NSString *msg = nil;
    
            if (nameField.text.length > 0) 
                msg = [[NSString alloc] initWithFormat:
                       @"You can breathe easy, %@, everything went OK.",
                       nameField.text];
            else
                msg = @"You can breathe easy, everything went OK.";
    
            UIAlertView *alert = [[UIAlertView alloc]
                                  initWithTitle:@"Something was done" //设置标题，将会显示在Alert的顶部
                                  message:msg //设置提示消息内容
                                  delegate:self //设置警告视图self委托
                                  cancelButtonTitle:@"Phew" //设置取消按钮的标题
                                  otherButtonTitles:nil];
                                  //otherButtonTitles: @"New Button 1", @"New Button 2", nil]; //可以设置任意多的确定按钮
                [alert show];
        }
    }

这时候Run一下我们的程序，点击分段控件，切换到Button，点击`Do Something` 按钮，会出现一个操作表，然后选择yes的时候除弹出一个警报。   **六、美化我们的按钮。**