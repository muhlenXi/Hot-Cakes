### UINavigationController 知识点

#### 0、实例化window && 显示window && 设置window的根视图控制器

```objc
self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];

UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:rootViewController];

self.window.rootViewController = navigationController;

[self.window makeKeyAndVisible];
```

#### 1、创建一个 navigationController

```objc
UINavigationController * firstNav = [[UINavigationController alloc] initWithRootViewController:firstVC];
```

#### 2、自定义BBI，侧滑手势生效

```objc
self.navigationController.interactivePopGestureRecognizer.delegate =(id)self;
```

#### 3、设置状态栏的颜色

```objc
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
```
   
#### 4、设置NavgationBar的字体大小和字体颜色

```objc
[[UINavigationBar appearance] setTitleTextAttributes: [NSDictionary dictionaryWithObjectsAndKeys:
    NavgationTitle_Color, NSForegroundColorAttributeName,S20Font, NSFontAttributeName, nil]];
```
    
#### 5、设置导航条颜色

```objc
[UINavigationBar appearance].barTintColor = NavgationBar_Color;
```
    
#### 6、设置导航条内容的颜色

```objc
[[UINavigationBar appearance] setTintColor:NavgationTitle_Color];
```
    
#### 7、设置导航栏不透明

```objc
navigationController.navigationBar.translucent = NO;
```

*隐藏导航条  64个点的高度*

```objc
navigationController.navigationBarHidden = YES;
```

#### 8、设置背景图片

```objc
UIImage *image = [UIImage imageNamed:@"navBar"];
[navigationController.navigationBar setBackgroundImage:image  forBarMetrics:UIBarMetricsDefault];
```

#### 9、创建UIBarButtonItem

*创建系统自带的UIBarButtonSystemItem*

```objc
UIBarButtonItem *leftItem2 = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemCamera target:self action:@selector(leftAction)];
```

*文字UIBarButtonItem的创建方式*

```objc
UIBarButtonItem *leftItem = [[UIBarButtonItem alloc] initWithTitle:@"左按钮" style:UIBarButtonItemStylePlain target:self action:@selector(leftAction)];
```

*图片UIBarButtonItem的创建方式*
		
```objc
UIImage *image = [UIImage imageNamed:@"detail_highlight"];
// 图片渲染模式，不渲染图片，使用原始样式
image = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
UIBarButtonItem *leftItem1 = [[UIBarButtonItem alloc] initWithImage:image  style:UIBarButtonItemStylePlain target:self action:@selector(leftAction)];
```

*使用自定义的视图创建*

*1)使用图片做CustomView*

```objc
UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(30,30, 50, 44)];
imageView.image = image;	//给imageView添加手势，获得点击事件
UIBarButtonItem *leftItem3 = [[UIBarButtonItem alloc] initWithCustomView:imageView];
```    
    
*2)使用按钮做CustomView*

```objc
UIButton *leftButton = [UIButton buttonWithType:UIButtonTypeSystem];
leftButton.frame = CGRectMake(0, 0, 44, 44);
[leftButton setTitle:@"hello" forState:UIControlStateNormal];
[leftButton addTarget:self action:@selector(leftAction) forControlEvents:UIControlEventTouchUpInside];

UIBarButtonItem *leftItem4 = [[UIBarButtonItem alloc] initWithCustomView:leftButton];
```

*将UIBarButtonItem加入到导航的左边和右边*

```objc
self.navigationItem.leftBarButtonItem = leftItem1;

self.navigationItem.rightBarButtonItem = leftItem2;
```

*返回按钮的事件不起作用*

```objc
UIBarButtonItem *backItem = [[UIBarButtonItem alloc] initWithImage:[UIImage  imageNamed:@"arrow_left_40"] style:UIBarButtonItemStylePlain target:nil  action:nil];	
self.navigationItem.backBarButtonItem = backItem;
```

*隐藏返回按钮*

```objc
self.navigationItem.hidesBackButton = YES;
```

#### 10、设置导航栏的标题

```objc
self.navigationItem.title = @"界面一";
```

#### 11、设置导航栏的titleView

```objc
 self.navigationItem.titleView = /* 自定义View */
```

#### 12、获取导航控制器中的视图数组

```objc
NSArray * arrays = [self.navigationController viewControllers];
```

#### 13、将视图控制器压入导航控制器的栈容器中

```objc
controller_name * vc = [[controller_name alloc] init];    
[self.navigationController pushViewController:vc animated:YES];
```

#### 14、将视图控制器从导航控制器中弹出

```objc
[self.navigationController popViewControllerAnimated:YES];
```

#### 15、切换至指定的视图控制器(该控制器必须在当前导航控制器中的栈中)

```objc
[self.navigationController popToViewController: vc_name animated:YES];
```
#### 16、回到根视图控制器

```objc
[self.navigationController popToRootViewControllerAnimated:YES];
```

#### 17、设置工具栏的内容

```objc
// 创建按钮  文字
UIBarButtonItem *item1 = [[UIBarButtonItem alloc] initWithTitle:@"item" style:UIBarButtonItemStylePlain target:self action:@selector(toolBarItemAction)];

// 图片
UIBarButtonItem *item2 =[[UIBarButtonItem alloc] initWithImage:[UIImage imageNamed:@"detail_highlight@2x"] style:UIBarButtonItemStylePlain target:self action:@selector(toolBarItemAction)];	

// 系统        
UIBarButtonItem *item3 = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemBookmarks target:self action:@selector(toolBarItemAction)];	
        
// 自定义
UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, 44, 44)];
label.backgroundColor = [UIColor lightGrayColor];
label.text = @"hello";
 UIBarButtonItem *item4 = [[UIBarButtonItem alloc] initWithCustomView:label];
        
// 创建空格
UIBarButtonItem *space = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFlexibleSpace target:nil  action:nil];
        
// 将按钮和空格加入到工具栏中
self.toolbarItems = @[space, item1, space, item2, space, item3, space, item4, space];
```