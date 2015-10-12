##3D Touch APIs
iOS 9提供了如下3D Touch APIs:

* **Home Screen quick action API**使你可以给应用图标添加快速选项,让你的应用预料和加速用户体验.
* **UIKit peek and pop API**让你在应用中提供快速访问更多内容的功能.使用peek quick actions的API提供的按压功能来替换你应用的触碰长按.
* **Web view peek and pop API**让你使用系统提供的HTML链接预览功能.
* **UITouch force properties**让你在应用增加自定义的基于压力的用户交互.

不管你使用以上哪些APIs,你的应用必须在运行时检测3D Touch的可用性.

###检测3D Touch的可用性

为了在运行时检测设别是否支持3D Touch,读取那些拥有特征环境(a trait environment,见[UITraitEnvironment](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitEnvironment_Ref/index.html#//apple_ref/doc/uid/TP40014306) Protocol Reference)的对象的特征集合(trait collection)的[forceTouchCapability](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/occ/instp/UITraitCollection/forceTouchCapability)属性值.由于用户在应用运行时可以关闭3D Touch,所以在你实现的[traitCollectionDidChange:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitEnvironment_Ref/index.html#//apple_ref/occ/intfm/UITraitEnvironment/traitCollectionDidChange:)代理方法中把读取这个属性的值作为其一部分.

为了确保你的用户能够完整使用你app的功能,请根据3D Touch是否可用来分别规划你的代码.当3D Touch可用时,尽可能利用它的能力.当它不能使用时候,提供可供替代的方案比如由UILongPressGestureRecognizer类实现的触碰和长按.

请参阅[iOS Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556)来增加你对提升应用交互的理解,保证为那些使用3D Touch设备的用户带来便利的同时,也不要怠慢其他的用户.

###主屏快速选项
iOS9 支持主屏静态和动态快速选项.

* **静态快速选项**(Static quick actions)当用户安装完应用后立即就能使用.在你应用的*Info.plist*文件中的[UIApplicationShortcutItems](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW36)数组中定义**静态快速选项**.
* **动态快速选项**(Dynamic quick actions)在用户第一次加载应用后可用.使用[UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/index.html#//apple_ref/occ/cl/UIApplicationShortcutItem), [UIMutableApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/index.html#//apple_ref/occ/cl/UIMutableApplicationShortcutItem)和[UIApplicationShortcutIcon](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/index.html#//apple_ref/occ/cl/UIApplicationShortcutIcon)类和相关的API来定义**动态快速选项**.使用[shortcutItems](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/instp/UIApplication/shortcutItems)属性来添加动态快速选项到你应用的[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)对象中.

iOS9中一个应用最多能展示四个主屏快速选项.在这个限制下,系统首先展示静态快速选项,以plist菜单中的第一个为首.如果你的静态选项不够四个并且你也定义了动态快速选项,那么一个或多个动态快速选项会被展现出来.

两种快速选项都可以显示最多两行文本和一个可选的图标.系统会格式化文本,排列包装它,并且适当的添加省略号.

主屏快速选项支持VoiceOver.

有关实现主屏快速选项的详细内容,查阅以下资料:

* [UIApplicationShortcutItem Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/index.html#//apple_ref/doc/uid/TP40016501)
* [UIMutableApplicationShortcutItem Class Reference
](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/index.html#//apple_ref/doc/uid/TP40016502)
* [UIApplicationShortcutIcon Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/index.html#//apple_ref/doc/uid/TP40016500)
* [UIApplicationShortcutItems](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW36) in [Information Property List Key Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)
* [ApplicationShortcuts: Using UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/Introduction/Intro.html#//apple_ref/doc/uid/TP40016545)(示例代码)


###UIKit中的Peek 和 Pop


iOS9让你可以配置视图控制器来给用户提供peek和pop.

为了在3D Touch设备上支持peek和pop,iOS9 SDK包括:

* 在UIViewController类中增加了一系列用来注册和注销一个参与3D Touch的视图控制器的新方法.
* 为了支持3D Touch的新视图控制器协议

你可以有选择的配置一个包括一系列peek快速选项的预览视图控制器.用户可以通过在peek界面上滑来使用peek快速选项.

为了支持peek快速选项,iOS9 SDK包括:

* 全新的UIPreviewAction和UIpreviewActionGroup类
* 全新的UIPreviewActionItem协议

有关实现peek和pop和实现peek快速选项的更多资料,查阅以下资料:

* [UIViewController Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/doc/uid/TP40006926)中对 [registerForPreviewingWithDelegate:sourceView:](registerForPreviewingWithDelegate:sourceView:)和[unregisterForPreviewingWithContext:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/instm/UIViewController/unregisterForPreviewingWithContext:)方法的描述.
* [UIViewControllerPreviewing Protocol Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewControllerPreviewing_Protocol/index.html#//apple_ref/doc/uid/TP40016568), 描述了开启3D Touch视图控制器的上下文对象的接口.
* [UIViewControllerPreviewingDelegate Protocol Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewControllerPreviewingDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40016569), 描述了响应用户压力触控的预览控制器(用户术语为peek)和响应更大的压力按下的提交控制器(用户术语为pop)的接口.
* [UIPreviewAction Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/index.html#//apple_ref/doc/uid/TP40016565), 描述了peek快速选项.
* [UIPreviewActionGroup Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/index.html#//apple_ref/doc/uid/TP40016566), 描述了peek快速选项的子菜单分组.
* [UIPreviewActionItem Protocol Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/index.html#//apple_ref/doc/uid/TP40016567), 描述了peek快速选项和分组的接口.
* [ViewControllerPreviews: Using the UIViewController previewing APIs](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html#//apple_ref/doc/uid/TP40016546) (示例代码)

###Web View的Peek和Pop

在web视图中,你可以对超链接和侦测到的数据使用全新的**allowsLinkPreview**属性来开启peek和pop.在iOS9中,这个属性在更被推荐使用的WKWebView类(属于WebKit framework)和较老的UIWebVIew类(属于UIKit framework)中都是可以使用的.

Peek和Pop会通过Safari Services framewrok中的SFSafariViewController(Safari view controller)类自动侦测链接和数据

###UITouch对象中的压力属性

在iOS9中,UITouch类获得了两个全新的属性帮助你在应用中自定义3D Touch:[force](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/instp/UITouch/force)和[maximumPossibleForce](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/instp/UITouch/maximumPossibleForce).这是第一次在iOS设备上,这些属性可以在你应用接收到的UIEvent对象中让你侦测并响应触控压力.

在iPhone上,触控压力感应有很大的动态范围,对于你的应用来说它是个浮点值.

有关更多自定义使用压力值对3D Touch的实现,阅读以下资料:

* 在[UITouch Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/doc/uid/TP40006785)中对[force](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/instp/UITouch/force)和[maximumPossibleForce](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/index.html#//apple_ref/occ/instp/UITouch/maximumPossibleForce)属性的描述.
* [TouchCanvas: Using UITouch efficiently and effectively
](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/Introduction/Intro.html#//apple_ref/doc/uid/TP40016561)(示例代码)






