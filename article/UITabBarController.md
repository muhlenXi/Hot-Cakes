---
title: UITabBarController 的详细运用
date: 2016-09-05 14:42:25
categories: blog
tags: [Objective-C,UITabBarController]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/09/05/UITabbarController](http://muhlenxi.com/2017/09/05/UITabbarController)*

### 导语：

> iPhone 和 iPod touch 都可以使用 UITabBarController 类，令用户能够在多个视图控制器之间切换，并且能够定制屏幕底部的标签栏。用户只需点击一下屏幕，就能切换到其他视图，而且还可以通过 More 按钮来选择并编辑需要显示在屏幕底部标签栏里的标签（Tab）。

  写个 demo 玩一下，实践是检验真理的唯一标准。

<!-- more -->

### 创建系统 Item

【1】在 AppDelegate 的 `didFinishLaunchingWithOptions` 方法中,我们初始化一个UITabBarController 和 11个 UIViewController ：代码如下

```objc
//1.初始化TabBarController
UITabBarController * tabbarC = [[UITabBarController alloc] init];
//2.初始化ViewControllers
FirstViewController * firstVC = [[FirstViewController alloc] initWithNibName:@"FirstViewController" bundle:nil];
UINavigationController * firstNav = [[UINavigationController alloc] initWithRootViewController:firstVC];
SecondViewController * secondVC = [[SecondViewController alloc] initWithNibName:@"SecondViewController" bundle:nil];
UINavigationController * secondNav = [[UINavigationController alloc] initWithRootViewController:secondVC];
ThirdViewController * thirdVC = [[ThirdViewController alloc] initWithNibName:@"ThirdViewController" bundle:nil];
UINavigationController * thirdNav = [[UINavigationController alloc] initWithRootViewController:thirdVC];
FourthViewController * fourthVC = [[FourthViewController alloc] initWithNibName:@"FourthViewController" bundle:nil];
UINavigationController * fourthNav = [[UINavigationController alloc] initWithRootViewController:fourthVC];
FifthViewController * fifthVC = [[FifthViewController alloc] initWithNibName:@"FifthViewController" bundle:nil];
UINavigationController * fifthNav = [[UINavigationController alloc] initWithRootViewController:fifthVC];
SixthViewController * sixthVC = [[SixthViewController alloc] initWithNibName:@"SixthViewController" bundle:nil];
UINavigationController * sixNav = [[UINavigationController alloc] initWithRootViewController:sixthVC];
SeventhViewController * seventhVC = [[SeventhViewController alloc] initWithNibName:@"SeventhViewController" bundle:nil];
UINavigationController * sevenNav = [[UINavigationController alloc] initWithRootViewController:seventhVC];
EighthViewController * eighthVC = [[EighthViewController alloc] initWithNibName:@"EighthViewController" bundle:nil];
UINavigationController * eightNav = [[UINavigationController alloc] initWithRootViewController:eighthVC];
NinthViewController * ninthVC = [[NinthViewController alloc] initWithNibName:@"NinthViewController" bundle:nil];
UINavigationController * nineNav = [[UINavigationController alloc] initWithRootViewController:ninthVC];
TenthViewController * tenVC = [[TenthViewController alloc] initWithNibName:@"TenthViewController" bundle:nil];
UINavigationController * tenNav = [[UINavigationController alloc] initWithRootViewController:tenVC];
EleventhViewController * elevenVC = [[EleventhViewController alloc] initWithNibName:@"EleventhViewController" bundle:nil];
UINavigationController * elevenNav = [[UINavigationController alloc] initWithRootViewController:elevenVC];
//3.设置viewControllers
NSArray * array = @[firstNav,secondNav,thirdNav,fourthNav,fifthNav,sixNav,sevenNav,eightNav,nineNav,tenNav,elevenNav];
tabbarC.viewControllers = array; 
tabbarC.customizableViewControllers = array;  //编辑模式
```

【2】分别在每个Controller的`.m`文件中重写 `initWithNibName` 方法，代码如下：

```objc
- (instancetype)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
{
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self)
     {
        //创建系统TabBarItem并设置
        UITabBarItem * item = [[UITabBarItem alloc] initWithTabBarSystemItem:UITabBarSystemItemMostViewed tag:0];
        self.tabBarItem = item;
    }
    return  self;
}
```

*注意：系统共有一下十二种样式的 TabbarItem*

    0 - UITabBarSystemItemMore,
    1 - UITabBarSystemItemFavorites,
    2 - UITabBarSystemItemFeatured,
    3 - UITabBarSystemItemTopRated,
    4 - UITabBarSystemItemRecents,
    5 - UITabBarSystemItemContacts,
    6 - UITabBarSystemItemHistory,
    7 - UITabBarSystemItemBookmarks,
    8 - UITabBarSystemItemSearch,
    9 - UITabBarSystemItemDownloads,
    10 - UITabBarSystemItemMostRecent,
    11 - UITabBarSystemItemMostViewed,

编译并运行代码，会出现如下的界面：

*如果视图控制器中的控制器的数量超过5个时，就会使用导航控制器管理剩余的视图控制器，并且将导航控制器作为第五个视图控制器。*

<div align = center>
![tabbar](http://7xvffo.com1.z0.glb.clouddn.com/tabbar.PNG)
</div>


### 设置 tabBarController 的代理。

*点击最后一个 More 标签，进入 More 界面，点击 Edit 后，长按拖动 Item，替换靠前的 Item，我们会发现，这时候 Tabbar 的 Item 的排列顺序发生了改变，记住此时的顺序。然后重新运行 APP，会发现则又恢复到了编辑前的顺序了，若要保存编辑的后顺序，我们需通过 NSUserDefaults 来保存排序后的数组*。

【1】设置代理

```objc
tabbarC.delegate = self;
```

【2】实现相应的代理方法

**点击 `选中` 会触发这两个方法**

```objc
 - (BOOL)tabBarController:(UITabBarController *)tabBarController shouldSelectViewController:(UIViewController *)viewController
{
    NSLog(@"tabBarController -- shouldSelectViewController"); 
    UINavigationController * nav = (UINavigationController *)viewController;
    if ([nav.topViewController isKindOfClass:[SecondViewController class]]) {
        //第二个tabbarItem不能选中
        return NO;
    }
    return YES;
}
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController
{
    NSLog(@"tabBarController --- didSelectViewController");
}
```

**点击 `编辑` 后会触发这个方法**

```objc
- (void)tabBarController:(UITabBarController *)tabBarController willBeginCustomizingViewControllers:(NSArray<__kindof UIViewController *> *)viewControllers
{
    NSLog(@"tabBarController --- willBeginCustomizingViewControllers");
}
```

**点击 `完成` 后会触发这两个方法**

```objc
- (void)tabBarController:(UITabBarController *)tabBarController willEndCustomizingViewControllers:(NSArray<__kindof UIViewController *> *)viewControllers changed:(BOOL)changed
{
    NSLog(@"tabBarController --- willEndCustomizingViewControllers");
}
```

*在这个方法中保存编辑后的 tabbarItem 的数组*

```objc
- (void)tabBarController:(UITabBarController *)tabBarController didEndCustomizingViewControllers:(NSArray<__kindof UIViewController *> *)viewControllers changed:(BOOL)changed
{
    NSLog(@"tabBarController --- didEndCustomizingViewControllers");
    //获取并保存
    NSMutableArray * endEditArray = [NSMutableArray array];
    for (UINavigationController * nav in viewControllers) { 
       NSString * name =  NSStringFromClass([nav.topViewController class]);
       [endEditArray addObject:name];
    }
    [[NSUserDefaults standardUserDefaults] setObject:endEditArray forKey:@"ItemArray"];
    [[NSUserDefaults standardUserDefaults] synchronize];  
}
```

【3】在 `didFinishLaunchingWithOptions` 方法中，修改tabbarC.viewControllers的设置。

修改如下：

```objc
UITabBarController * tabbarC = [[UITabBarController alloc] init];
    tabbarC.delegate = self;
NSMutableArray * getItemArray = [[NSUserDefaults standardUserDefaults] objectForKey:@"ItemArray"];
    if (getItemArray == nil)
    {
       FirstViewController * firstVC = [[FirstViewController alloc] initWithNibName:@"FirstViewController" bundle:nil];
        UINavigationController * firstNav = [[UINavigationController alloc] initWithRootViewController:firstVC];
        SecondViewController * secondVC = [[SecondViewController alloc] initWithNibName:@"SecondViewController" bundle:nil];
        UINavigationController * secondNav = [[UINavigationController alloc] initWithRootViewController:secondVC];
        ThirdViewController * thirdVC = [[ThirdViewController alloc] initWithNibName:@"ThirdViewController" bundle:nil];
        UINavigationController * thirdNav = [[UINavigationController alloc] initWithRootViewController:thirdVC];
        FourthViewController * fourthVC = [[FourthViewController alloc] initWithNibName:@"FourthViewController" bundle:nil];
        UINavigationController * fourthNav = [[UINavigationController alloc] initWithRootViewController:fourthVC];
        FifthViewController * fifthVC = [[FifthViewController alloc] initWithNibName:@"FifthViewController" bundle:nil];
        UINavigationController * fifthNav = [[UINavigationController alloc] initWithRootViewController:fifthVC];
        SixthViewController * sixthVC = [[SixthViewController alloc] initWithNibName:@"SixthViewController" bundle:nil];
        UINavigationController * sixNav = [[UINavigationController alloc] initWithRootViewController:sixthVC];
        SeventhViewController * seventhVC = [[SeventhViewController alloc] initWithNibName:@"SeventhViewController" bundle:nil];
        UINavigationController * sevenNav = [[UINavigationController alloc] initWithRootViewController:seventhVC];
        EighthViewController * eighthVC = [[EighthViewController alloc] initWithNibName:@"EighthViewController" bundle:nil];
        UINavigationController * eightNav = [[UINavigationController alloc] initWithRootViewController:eighthVC];
        NinthViewController * ninthVC = [[NinthViewController alloc] initWithNibName:@"NinthViewController" bundle:nil];
        UINavigationController * nineNav = [[UINavigationController alloc] initWithRootViewController:ninthVC];
        TenthViewController * tenVC = [[TenthViewController alloc] initWithNibName:@"TenthViewController" bundle:nil];
        UINavigationController * tenNav = [[UINavigationController alloc] initWithRootViewController:tenVC];
        EleventhViewController * elevenVC = [[EleventhViewController alloc] initWithNibName:@"EleventhViewController" bundle:nil];
        UINavigationController * elevenNav = [[UINavigationController alloc] initWithRootViewController:elevenVC];
       NSArray * array = @[firstNav,secondNav,thirdNav,fourthNav,fifthNav,sixNav,sevenNav,eightNav,nineNav,tenNav,elevenNav];
        getItemArray = [NSMutableArray arrayWithArray:array];
    }
    else
    {
        NSMutableArray * items = [NSMutableArray array];
        for (NSString * name in getItemArray) {
             //根据记录的类名创建
            UIViewController *vc = [[NSClassFromString(name) alloc] init];
            UINavigationController * nav = [[UINavigationController alloc] initWithRootViewController:vc];
            [items addObject:nav];
        }
        getItemArray = [[NSMutableArray alloc] initWithArray:items];
    }
    tabbarC.viewControllers = getItemArray;
    tabbarC.customizableViewControllers = getItemArray;  //编辑模式
```

*运行代码后，编辑并保存后。关闭 APP，再打开，会发现是编辑后的顺序了*

### NavigationBar 和 Tabbar 的外观设置

*这里是 NavigationBa r的外观的全局设置*

```objc
    //设置状态栏的字体颜色
    [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
 	//设置NavgationBar的字体大小和字体颜色
    [[UINavigationBar appearance] setTitleTextAttributes: [NSDictionary dictionaryWithObjectsAndKeys:
    NavgationTitle_Color, NSForegroundColorAttributeName,S20Font, NSFontAttributeName, nil]];
    //设置NavgationBar的背景颜色
    [UINavigationBar appearance].barTintColor = NavgationBar_Color;
    //设置NavgationBar的Item的颜色
    [[UINavigationBar appearance] setTintColor:NavgationTitle_Color];
    //设置导航栏不透明
    [UINavigationBar appearance].translucent = NO;
```
 
 *这里是 TabBar 的外观的全局设置*

```objc   
    //设置标题栏不透明
    [UITabBar appearance].translucent = NO;
    //设置标签栏的背景颜色
    [UITabBar appearance].barTintColor = TabBar_Color;
    //未选中字体颜色
    [[UITabBarItem appearance] setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:TabBarItem_DisselectedColor,NSForegroundColorAttributeName,S12Font,NSFontAttributeName, nil] forState:UIControlStateNormal];
    //选中字体颜色
    [[UITabBarItem appearance] setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:TabBarItem_SelectedColor,NSForegroundColorAttributeName,S12Font,NSFontAttributeName, nil] forState:UIControlStateSelected];
```

### 一般TabbarItem 的创建

【1】在AppDelegate的 `didFinishLaunchingWithOptions` 方法中,我们初始化一个UITabBarController 和4个 UIViewController,并做一些简单的设置就可以了，代码如下：

```objc
    //one
    OneViewController * oneVC = [[OneViewController alloc]init];
    UINavigationController * oneNav = [[UINavigationController alloc]initWithRootViewController:self.oneVC];
    //two
    TwoViewController * twoVC = [[TwoViewController alloc]init];
    UINavigationController * twoNav = [[UINavigationController alloc]initWithRootViewController:self.twoVC];
    //three
    ThreeViewController * threeVC = [[ThreeViewController alloc]init];
    UINavigationController * threeNav = [[UINavigationController alloc]initWithRootViewController:self.threeVC];
    //four
    FourViewController * fourVC = [[FourViewController alloc]init];
    UINavigationController *fourNav = [[UINavigationController alloc]initWithRootViewController:self.fourVC];
    //加TabBarController
    UITabBarController *tabBarController = [[UITabBarController alloc] init];
    tabBarController.viewControllers = @[oneNav,twoNav,threeNav,fourNav];
    //设置tabbar相关属性
    UITabBar *tabBar = tabBarController.tabBar;
    UITabBarItem *oneItem  = [tabBar.items objectAtIndex:0];
    UITabBarItem *twoItem  = [tabBar.items objectAtIndex:1];
    UITabBarItem *threeItem = [tabBar.items objectAtIndex:2];
    UITabBarItem *fourItem = [tabBar.items objectAtIndex:3];
    //设置标题
    oneItem.title = @"One";
    twoItem.title = @"Two";
    threeItem.title = @"Three";
    fourItem.title = @"Four";
    //设置图片
    //设置一般图片
    [oneItem setImage:[[UIImage imageNamed:@"tabbar_contacts"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal ]];
    //设置选中照片
    [oneItem setSelectedImage:[[UIImage imageNamed:@"tabbar_contactsHL"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal ]];
    [twoItem setImage:[[UIImage imageNamed:@"tabbar_discover"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
    [twoItem setSelectedImage:[[UIImage imageNamed:@"tabbar_discoverHL"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
    [threeItem setImage:[[UIImage imageNamed:@"tabbar_mainframe"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
    [threeItem setSelectedImage:[[UIImage imageNamed:@"tabbar_mainframeHL"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
    [fourItem setImage:[[UIImage imageNamed:@"tabbar_me"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
    [fourItem setSelectedImage:[[UIImage imageNamed:@"tabbar_meHL"]imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
    //设置window的跟视图控制器
    self.window.rootViewController = tabBarController;
```

编译并运行代码，会出现如下的界面：

<div align = center>
![tabbar](http://7xvffo.com1.z0.glb.clouddn.com/tabbar2.PNG)
</div>

### 自定义Tabbar

【1】新建一个继承于 `XYJTabBarController` 的 `XYJTabBarController` 控制器：

打开`.m`文件，在类扩展中，定义一个 `UIImageView` 的属性 `customTabbar`,代码如下:

```objc
@property (nonatomic,strong) UIImageView * customTabbar;  //!< 自定义Tabbar ImageView
```

【2】在 `viewDidLoad` 中初始化相关页面

```objc
//创建要放在TabBarController中的视图控制器
    //one
    OneViewController * oneVC = [[OneViewController alloc] init];
    UINavigationController * oneNav = [[UINavigationController alloc] initWithRootViewController:oneVC];
    //two
    TwoViewController * twoVC = [[TwoViewController alloc] init];
    UINavigationController * twoNav = [[UINavigationController alloc] initWithRootViewController:twoVC];
    //three
    ThreeViewController * threeVC = [[ThreeViewController alloc] init];
    UINavigationController * threeNav = [[UINavigationController alloc] initWithRootViewController:threeVC];
    //four
    FourViewController * fourVC = [[FourViewController alloc] init];
    UINavigationController * fourNav = [[UINavigationController alloc] initWithRootViewController:fourVC];
	//设置viewControllers
    self.viewControllers = @[oneNav,twoNav,threeNav,fourNav];
    //自定义tabbar
    //1.隐藏系统的tabbar
    self.tabBar.hidden = YES;
    //2.初始化customTabbar
    self.customTabbar = [[UIImageView alloc] initWithFrame:CGRectMake(0, self.view.bounds.size.height-49, self.view.bounds.size.width, 49)];
    self.customTabbar.image = [UIImage imageNamed:@"tabbar_bg"];
    //打开用户交互
    self.customTabbar.userInteractionEnabled = YES;
    [self.view addSubview:self.customTabbar];
    //3.给tabbar添加按钮
    CGFloat width = self.view.bounds.size.width / 4;
    //去沙盒读取状态选择
    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
    NSNumber *index = [userDefaults objectForKey:@"tabbarSelectedIndex"];
    if (index == nil) {
        //默认选中第一个Item
        index = @(0);
    }
    for (int i = 0; i < 4; i++)
    {
        //创建按钮
        UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];
        btn.frame = CGRectMake(width * i, 0, width, 49);
        //设置正常状态的图片
        UIImage *image = [UIImage imageNamed:[NSString stringWithFormat:@"tab%d_n@2x", i+1]];
        [btn setImage:image forState:UIControlStateNormal];
        //设置选中状态的图片
        UIImage *selectImage = [UIImage imageNamed:[NSString stringWithFormat:@"tab%d_s@2x", i+1]];
        [btn setImage:selectImage forState:UIControlStateSelected];
        if (i == index.integerValue)
        {
            btn.selected = YES;  //设置选中状态
        }
        //设置tag
        btn.tag = i + 100;
        //添加事件，改变选中状态
        [btn addTarget:self action:@selector(btnAction:) forControlEvents:UIControlEventTouchUpInside];
        [self.customTabbar addSubview:btn];
     }
```

【3】在AppDelegate的 `didFinishLaunchingWithOptions` 方法中,我们初始化自定义的TabBarController，并将其设置为 Window 的rootViewController：代码如下

```objc
XYJTabBarController * tabbarController = [[XYJTabBarController alloc] init];
self.window.rootViewController = tabbarController;
```

编译并运行代码，会出现如下的界面：

<div align = center>
![tabbar](http://7xvffo.com1.z0.glb.clouddn.com/tabbarcustom.PNG)
</div>

*感谢阅读，TabarController的学习暂时到这里为止*
