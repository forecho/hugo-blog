---
title: "iOS开发实例（五）- 自动旋转"
date: 2012-11-28T15:58:00+08:00
categories: 
draft: false
toc: true
---

自动旋转屏幕有3种常用方法： **一、自动调整属性（适用比较简单的界面）。** **       **1、创建一个项目`Autosize`，系统是默认支持`Portrait`（纵版）、`Landscape Left`（向左横向）、`Landscape Right`（向右横向），不支持`Upside Down`（倒过来），如下图：** ** **![](http://m2.img.libdd.com/farm4/2012/1121/23/3AE597CB846B2D7F4088044E43CA64F30E8F6AB6F1AC6_479_131.PNG) ** 2、打开.xib文件，在IB中拖出6个Round Rect Button，按下图摆放和命名： ![](http://m1.img.libdd.com/farm4/2012/1121/23/8AD91C50170E0A62231370F87495642C190C42E210C75_354_516.PNG) 这个时候Run一下程序，然后在菜单处找到硬件->向左旋转。然后得到如下图结果： ![](http://m1.img.libdd.com/farm5/2012/1121/23/183A9CB8272F0D3BE0AC8236D1884596B96F4860DE0B8_716_363.PNG) 你会发现只有UL位置是对的，其他的都有问题。 3、选中按钮，在属性处找到大小检查器，如下图： ![](http://m2.img.libdd.com/farm4/2012/1121/23/9E85DA47E72390220DEEE923E5E0F5F14316732D21E0B_264_297.PNG) 方块里面箭头（→）：实线表示可在调整窗口大小时自由更改对象的宽度，虚线表示将对象尽可能的保持原始值。 方块周围的“I”形：表示选定对象的边与包含它的视图的同侧边之间的距离。虚线表示距离是可以灵活可变的，实线表示间距的指应尽可能的保持不变。 4、根据上面的规则修改，如下图： ![](http://m3.img.libdd.com/farm5/2012/1121/23/8E37B775DD89797ECD128C0575CC3F1F1119DD31E9728_259_267.PNG)![](http://m1.img.libdd.com/farm4/2012/1121/23/8C6A2DFC4F266FD1457629F9874C057EB88E51293E916_259_267.PNG)![](http://m1.img.libdd.com/farm4/2012/1121/23/9471BF6FA41620297854F761F7E9C181BF109E6EA8E5A_259_267.PNG)![](http://m2.img.libdd.com/farm5/2012/1121/23/3F1D573B73A663CFF17E04EB5FAA115CB6FEDB498342C_259_267.PNG)![](http://m1.img.libdd.com/farm4/2012/1121/23/53960D31310BAD0569F508E555A29C3E5AC96771F72AD_259_267.PNG)![](http://m2.img.libdd.com/farm4/2012/1121/23/750CD4749D564A2C925C021A6A6943037C0734F3ECF6F_259_267.PNG) 然后我们在Run一下程序，然后依次点击硬件->向左旋转，如果得到如下图结果，那就证明你成功了。 ![](http://m1.img.libdd.com/farm5/2012/1121/23/63FA6072C2B8DD53ACC858E9BDACF5C75FB7716FDCC31_718_363.PNG)   **二、看到视图旋转提示时，手动调整视图中的对象位置。** 1、选中6个按钮，同时把他们的高度和宽度设置成125点，然后效果图如下： ![](http://m3.img.libdd.com/farm5/2012/1125/02/28D38E6AF23163A7DE3654562A0B99A859569CC8942F8_355_516.PNG) 2、这个时候我们Run一下程序，然后旋转就会得到如下面的效果： ![](http://m2.img.libdd.com/farm5/2012/1125/02/E194DDFFDC3CE0C0A28CF24CE741388A140EEF803B254_715_368.PNG) 好混乱，感觉不会再看了。这样的问题要怎么样去解决呢？ 3、手动调整视图位置：按住control键分别把6个按钮拖到.h文件的@end前，依次分别命名为：`buttonUL`，`buttonUR`，`buttonL`，`buttonR`，`buttonLL`，`buttonLR`。生成的代码如下： 
    
    
    @property (weak, nonatomic) IBOutlet UIButton *buttonUL;
    @property (weak, nonatomic) IBOutlet UIButton *buttonUR;
    @property (weak, nonatomic) IBOutlet UIButton *buttonL;
    @property (weak, nonatomic) IBOutlet UIButton *buttonR;
    @property (weak, nonatomic) IBOutlet UIButton *buttonLL;
    @property (weak, nonatomic) IBOutlet UIButton *buttonLR;

4、在旋转时移动代码：在.m文件@end前写一个旋转开始之后的方法，最后的旋转动画发生之前自动调用。代码如下： 
    
    
    -(void)willAnimateRotationToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation duration:(NSTimeInterval)duration{
        if(UIInterfaceOrientationIsPortrait(interfaceOrientation)){
            buttonUL.frame = CGRectMake(20, 20, 125, 125);
            buttonUR.frame = CGRectMake(175, 20, 125, 125);
            buttonL.frame = CGRectMake(20, 168, 125, 125);
            buttonR.frame = CGRectMake(175, 168, 125, 125);
            buttonLL.frame = CGRectMake(20, 315, 125, 125);
            buttonLR.frame = CGRectMake(175, 315, 125, 125);
        }else{
            buttonUL.frame = CGRectMake(20, 20, 125, 125);
            buttonUR.frame = CGRectMake(20, 155, 125, 125);
            buttonL.frame = CGRectMake(177, 20, 125, 125);
            buttonR.frame = CGRectMake(177, 155, 125, 125);
            buttonLL.frame = CGRectMake(328, 20, 125, 125);
            buttonLR.frame = CGRectMake(328, 155, 125, 125);
        }
    }

所有的视图的大小位置都在frame属性中指定，`CGRectMake`函数支持通过指定的x和y的位置以及`width`和`height`来轻松创建`CGRect`。然后我们Run一下程序，得到如下效果： ![](http://m2.img.libdd.com/farm5/2012/1125/02/AAA523B63744F1C1B8F38BD5362C6886DE825EA56E99F_715_368.PNG) 其实这样做的有点复杂的。   **三、在IB中为视图设计两个不同的版本，一个适用于纵版，一个适用于横板。** 1、需要我们新建一个`Swap`项目。我们需要在nib文件中添加两个视图。默认的就是第一个视图，然后我们可以按住 option 键复制另一份视图，然后在属性检查器中的 `Simulated Metrics` 找到`Orientation` 菜单把 `Portrait` 改为 `Landscape` ，如下图所示： ![](http://m1.img.libdd.com/farm4/2012/1127/22/C808D665F6C2921F6B7B1E3A3805876465940AF1EE675_256_156.PNG) 2、创建两个视图：打开辅助编辑器，按住 Control 把视图拖到.h文件中，创建一名为 `portrait `的输出口，Storage 为 `Strong`，如下图： ![](http://m2.img.libdd.com/farm5/2012/1127/22/50C08C98ADDA5D5C576339688E0D09CA69A7AC6FB5366_267_152.PNG) 为横向视图重复以上操作，创建一名为 landscape 的输出口。 3、在IB库中拖出Round Rect Buttons，分别放入每个视图中，然后是大小检查器中将Width和Height属性改为125，然后移动位置，并且将标签改为`Foo`和 `Bar`。结果如下图所示： ![](http://m2.img.libdd.com/farm5/2012/1127/22/F3767BA302E80F7B350728D7B31A72A87AA574BC38925_800_467.jpg) 4、创建和关联按钮的输出口：按住control 把横向视图中的Foo按钮拖到.h文件中，将Connection 弹出菜单的值从Outlet 改为`Outlet `Collection，并且命名为`foos`。从纵向视图中的Foo 按钮拖至已存在的foos 输出口，与之关联。 对Bar 按钮进行以上重复动作，命名为`bars`。