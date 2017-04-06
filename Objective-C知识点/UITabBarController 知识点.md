### UITabBarController 知识点

#### 1、设置标题栏不透明

```objc
[UITabBar appearance].translucent = NO;
```
    
#### 2、设置标签栏的背景颜色

```objc
[UITabBar appearance].barTintColor = TabBar_Color;
```
    
#### 3、设置未选中字体颜色

```objc
[[UITabBarItem appearance] setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:TabBarItem_DisselectedColor,NSForegroundColorAttributeName,S12Font,NSFontAttributeName, nil] forState:UIControlStateNormal];
```
    
#### 4、设置选中字体颜色

```objc
[[UITabBarItem appearance] setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:TabBarItem_SelectedColor,NSForegroundColorAttributeName,S12Font,NSFontAttributeName, nil] forState:UIControlStateSelected];
```

#### 5、UITabBarController 的创建

```objc
UITabBarController *tabBarController = [[UITabBarController alloc] init];
```
#### 6、添加viewController

```objc
tabBarController.viewControllers = @[locationNav,ServiceNav,manageNav];
```

#### 7、设置tabbar Item 的标题

```objc
UITabBar *tabBar = tabBarController.tabBar;
UITabBarItem *locationItem  = [tabBar.items objectAtIndex:0];
locationItem.title = @"定位";
```
    
#### 8、设置tabbar Item 的图片和选中图片

```objc
locationItem.image = [[UIImage imageNamed:@"tab_location"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];

locationItem.selectedImage = [[UIImage imageNamed:@"tab_location_sel"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
```
#### 9、设置UITabBarItem徽标颜色和数值

```objc
locationItem.badgeColor = [UIColor redColor];
locationItem.badgeValue = @"5";
```

#### 10、设置选中项

```objc
tabBarController.selectedIndex = 0;
```    

#### 11、UITabBarController的Delegate

* 1、被选中的时候
 
```objc
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController
```

* 2、控制TabBarItem能不能选中
 
```objc
- (BOOL)tabBarController:(UITabBarController *)tabBarController shouldSelectViewController:(UIViewController *)viewController;
```

*3、下面这三个方法主要用于监测对moreViewController中对view controller的edit操作

```objc
- (void)tabBarController:(UITabBarController *)tabBarController willBeginCustomizingViewControllers:(NSArray *)viewControllers;

- (void)tabBarController:(UITabBarController *)tabBarController  willEndCustomizingViewControllers:(NSArray *)viewControllers changed:(BOOL)changed;
   
- (void)tabBarController:(UITabBarController *)tabBarController didEndCustomizingViewControllers:(NSArray *)viewControllers  changed:(BOOL)changed;
```
