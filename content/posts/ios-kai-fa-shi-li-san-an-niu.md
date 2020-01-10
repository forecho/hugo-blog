---
title: "iOS开发实例（三）-“按钮”"
date: 2012-10-23T23:42:00+08:00
categories: 
draft: false
toc: true
---

如果第4步不能直接关联了，因为是个 UIview，没有动作，那么你需要在按钮上右键，然后点住Touch Up Inside再拖到辅助编辑器中间位置。 ================以上为Update 2013年11月23日================== 与用户交互是应用程序的一种重要功能。 我们首先按照实例一的步骤创建一个新的项目-----》Button Fun。 项目创建好之后的文件除了项目名不同，其他的应该和实例一的截图一样。   1、首先我们需要清理一下试图控制器（简化代码，删除没必要的代码）。找到BIDViewController.m文件，源代码如下图： ![](http://m1.img.libdd.com/farm4/2012/1023/22/9FC37F7A1C38432DC0ADA9818D89142D3968442334835_500_287.jpg) 删减之后的代码如下： 
    
    
    #import "BIDViewController.h"
    @implementation BIDViewController
    
    - (void)viewDidUnload
    {
        [super viewDidUnload];
        // Release any retained subviews of the main view.
        //e.g.self.myOutlet = nil;
    }
    
    @end

2、设计用户界面：点击BIDViewController.xib文件。可以根据之前的实例一来操作，我们找到Round Rect Button，然后直接按住鼠标左键拖到界面，放到适合的位置，然后命名为”Left“，如下图操作： ![](http://m3.img.libdd.com/farm5/2012/1023/22/5CB6F33D253FA65FF21006A1611FEA2616FEBC4281EAE_500_422.jpg) 3、开启辅助编辑器。找到编辑器右上角的Editor，点击一下中间的图标即为开启辅助编辑模式。 4、然后我们按住键盘的control键同时按住鼠标左键把按钮拖到辅助编辑器中间位置，如下图所示： ![](http://m1.img.libdd.com/farm4/2012/1023/22/A6185E7766D74EA283C9CABC46C7BE141CFEA50F59E4F_500_250.jpg) 然后会出现一个弹出框，我们把第一项改为“Action”（Action为操作方法，默认的Outlet为输出口），其他的可以按照下图去修改： ![](http://m3.img.libdd.com/farm4/2012/1023/22/ED68C529ABBC5DA3B0F78BAD7A562045C00C58990A895_500_145.jpg) 然后确定，Xcode会自动生成一行如下代码： 
    
    
    - (IBAction)buttonPressed:(UIButton *)sender;

5、然后我们再去添加另外一个按钮，把这个按钮命名为“Right”，拖到相应的位置；然后按住control键和鼠标左键，把Right按钮拖到之前的代码上面，如下图： ![](http://m3.img.libdd.com/farm5/2012/1023/23/57DF4F5E02BD8AFFC108A1405A4D64D504AFA3E83079A_500_312.jpg) 这样操作Xcode会自动关联该按钮和已存在的操作。 6、添加一个标签和输出口。添加一个Label到按钮上方，并且去掉其中的字，拉长与两边按钮对其，在如下图位置设置文字居中： ![](http://m3.img.libdd.com/farm5/2012/1023/23/15F98628570FADCF2861AE5D67BC6D1A5C1F6C441845B_262_412.PNG) 然后也是按住control键和鼠标左键拖到Label到BIDViewController.h文件中间，并且修改弹出框，如下图操作： ![](http://m1.img.libdd.com/farm5/2012/1023/23/5E5086BB8B73710631B810FEAE192C6AD6EE8842104A2_500_177.jpg) 然后确定，Xcode会自动生产一行代码： 
    
    
    @property (strong, nonatomic) IBOutlet UILabel *statusText;

6、这个时候我们的BIDViewController.m文件会多了两行代码，变成如下所示： ![](http://m2.img.libdd.com/farm5/2012/1023/23/68C5E48732A3BBB74EE74D0628268460B4506450D5929_387_205.PNG) 然后创建buttonPressed方法，相应的代码添加之后为： 
    
    
    - (IBAction)buttonPressed:(UIButton *)sender {
        NSString *title = [sender titleForState:UIControlStateNormal];
        statusText.text = [NSString stringWithFormat:@"%@ button pressed.",title];
    }
    @end

    OK，程序基本上算是完成了，这个时候你可以Run一下，然后默认情况下是两个按钮，然后你点击按钮会出现相应的文字提示。如果你看到下图，那么代表你成功了。 ![](http://m3.img.libdd.com/farm5/2012/1023/23/C8ECB9E82D66050088B8FB16A854AA8578EACB73EEFE2_368_716.PNG) \----本节完----