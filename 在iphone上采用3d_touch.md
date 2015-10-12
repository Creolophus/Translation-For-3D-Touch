#在iPhone上采用3D Touch
##3D Touch入门  

在iOS9中,新的iPhone机型将第三维度添加到用户界面  

* 用户现在可以用力摁下主屏按钮来快速调出应用提供的功能菜单
* 在应用中,用户现在可以用力摁下视图以查看更多内容的预览并且快速访问一些功能.
  
想查阅示例代码的话,可以下载下面的Xcode工程:

* [ApplicationShortcuts: Using UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/Introduction/Intro.html#//apple_ref/doc/uid/TP40016545),示范了主屏静态和动态快速选项.
* [ViewControllerPreviews: Using the UIViewController previewing APIs](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html#//apple_ref/doc/uid/TP40016546),示范了**peek**(预览)和**pop**(提交),以及peek快速选项.
* [TouchCanvas: Using UITouch efficiently and effectively](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/Introduction/Intro.html#//apple_ref/doc/uid/TP40016561),示范了在[UITouch](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/cl/UITouch)类中的新的压力属性.  

在你开始使用前,请阅读在[iOS Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556)中的[3D Touch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/MobileHIG/3DTouch.html#//apple_ref/doc/uid/TP40006556-CH71)

###主屏快速选项
  
用户早已习惯了点击一个应用按钮来打开它,或者长按任何应用来编辑主屏.现在,通过按下iPhone 6s或者iPhone 6s Plus上的一个应用按钮,用户获得了一系列*快速选项*.当用户选择了一个快速选项,你的应用启动或者加载,并且应用的delegate对象接收快速选项的消息.

![image](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/Art/maps_directions_home_2x.png)最棒的快速选项预料并且加速你应用的用户交互.iOS9 SDK提供了定义静态或者动态快速选项的API,仅对使用最新iPhone机型的用户有效.

* 在你应用的*Info.plist*文件中的[UIApplicationShortcutItems](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW36)数组中定义**静态快速选项**.
* 使用[UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/index.html#//apple_ref/occ/cl/UIApplicationShortcutItem)类和相关的API来定义**动态快速选项**.使用[shortcutItems](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/instp/UIApplication/shortcutItems)属性来添加动态快速选项到你应用的[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)对象中.

两种快速选项都可以显示最多两行文本和一个可选的图标.

###Peek和Pop

现在你可以授权给你应用的视图控制器来响应用户不同的按压力量.随着用户按压力量的增加,交互会出现三个阶段:  



1. 暗示内容预览是可使用的  
2. 展示预览(*peek*)，和快捷选项菜单（*peek quick actions*）
3. 可以选择跳转到预览中的视图(*pop*)

当你使用*peek*和*pop*时,系统通过压力决定从哪个阶段过度至下一个.用户可以在设置>通用>辅助功能>3D Touch中进行修改.

![image](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/Art/preview_available_2_2x.png)
#####暗示peek是可使用的

轻按后,周围内容会变得模糊,这告诉用户预览更多内容(*peek*)是可以使用的.

![image](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/Art/peek_2x.png)

#####Peek
稍微用力按下,屏幕视图就会过渡到peek,一个你设置的用来展示更多内容的视图－就像邮件应用做的一样.
如果用户这时结束了触碰,peek就会消失并且应用回到交互开始之前的状态.
或者这个时候,用户可以在peek界面上更用力按下来跳转到使用peek呈现的视图,这个过渡动画会使用系统提供的pop过渡效果.pop出来的视图会填满你应用的根视图并且显示一个返航按钮可以回到交互开始的地方.(图中没有显示最后展示pop视图的阶段)

![image](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/Art/peek_quick_actions_2x.png)

#####Peek快速选项
如果用户一直保持触摸,可以向上滑动Peek视图,系统会展示出你预先设置和peek关联的peek快速选项.
每一项peek快速选项都是你应用的深度链接.当peek快速选项出现后,用户可以停止触摸而且peek会停留在屏幕中.这允许用户点击一个快速选项,唤起相关链接.


你同样也可以在网页视图中开启peek和pop,请参看[Web View Peek and Pop](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/3DTouchAPIs.html#//apple_ref/doc/uid/TP40016543-CH4-SW5).


###压力属性
在iOS9中,UITouch类获得了两个全新的属性帮助你在应用中自定义3D Touch:[force](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/instp/UITouch/force)和[maximumPossibleForce](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/instp/UITouch/maximumPossibleForce).这是第一次在iOS设备上,这些属性可以在你应用接收到的UIEvent对象中让你侦测并响应触控压力.
触控压力感应有很大的动态范围,对于你的应用c来说它是个浮点值.

###3D Touch的辅助功能和人机界面指南

为了确保你的用户能够完整使用你app的功能,请根据3D Touch是否可用来分别规划你的代码.请参看[Checking for 3D Touch Availability](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Adopting3DTouchOniPhone/3DTouchAPIs.html#//apple_ref/doc/uid/TP40016543-CH4-SW2).

当3D Touch可用时,尽可能利用它的能力.当它不能使用时候,提供可供替代的方案比如触摸和长按.

3D Touch支持VoiceOver.想了解关于VoiceOver,查阅[Accessibility Programming Guide for iOS](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008785).

想获得更多关于3D Touch的重要指南,查阅[iOS Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556)中的[3D Touch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/MobileHIG/3DTouch.html#//apple_ref/doc/uid/TP40006556-CH71).

###开发环境
Xcode7支持3D Touch开发.所有Xcode的调试功能对3D Touch的新功能都是可使用的.

注意下列事项:

* 在Xcode7.0中你必须在真机上调试来开发3D Touch.Xcode7.0中的模拟器不支持3D Touch.
* 在Xcode7.0中你必须通过代码实现peek和pop视图控制器.Xcode7.0中的Interface Builder不提供图形支持以设置3D Touch的视图控制器转场.

请在3D Touch可使用和不可用两种情况下都测试你的应用,保证对所有用户都拥有相同功能.在一台支持3D Touch的设备上,你可以在设置>通用>辅助功能>3D Touch中关闭3D Touch.

