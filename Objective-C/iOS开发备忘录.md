### 配置备忘

#### Xcode代码块路径

* **/Users/`username`/Library/Developer/Xcode/UserData/CodeSnippets**

#### Objective-C 调用 Swift

【1】确保将项目的`Target`的 `Build Setting -> Packaging -> Defines Module` 设置为 `YES`。
【2】修改`Build Setting` 中的 `Product Module Name` 为 `项目名`。
【3】在调用的地方 导入 `#import <项目名-Swift.h>`即可。

#### pch文件路径

*$(SRCROOT)/项目名称/pch文件名*

#### 自动布局AutoLayout注意事项

*首先创建视图，然后将其添加到上级视图里，接下来禁用translatesAutoresizingMaskIntoConstraints,最后运用必要的约束规则。*

#### Xcode8控制台输出信息过多解决方法

Product -> Scheme -> Edit Scheme... -> Run -> Arguments, 在Environment Variables里边添加 OS_ACTIVITY_MODE ＝ disable

### 代码备忘


#### 声明代理协议

```objc
//定义代理协议
@protocol XYJWeatherAlertDelegate <NSObject>  //协议名称

@optional

- (void) XYJWeatherAlertViewCancel;  //代理方法

@end
```


#### 抛异常

```objc
@throw [NSException exceptionWithName:@"单例" reason:@"Use +[XYJItemsStore sharedStore]" userInfo:nil];
```

#### 识别点击手势之前避免向UIView发送touchesBegan消息

```objc
doubleTapRecognizer.delaysTouchesBegan = YES;
```

#### 弱引用

```
__weak typeof(self)  weakSelf = self;
```


#### 打开或关闭网络活动指示器

```objc
[UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
[UIApplication sharedApplication].networkActivityIndicatorVisible = NO;
```

 
#### 获取AppDelegate

```objc
+ (AppDelegate *)mainDelegate 
{
    return (AppDelegate *)[UIApplication sharedApplication].delegate;
}
```    
#### 获取APP的名称

```objc
 //获取App的名成
 NSString *app_Name = [[NSBundle mainBundle] objectForInfoDictionaryKey:@"CFBundleDisplayName"];
 NSLog(@"app_Name == %@",app_Name);
```
 


#### 判断字符串是否是纯数字

```objc
- (BOOL) stringIsNumber:(NSString *) str
{
    //判断字符串是否是纯数字
    NSString *regex =@"[0-9]*";
    NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF MATCHES %@",regex];
    if ([pred evaluateWithObject:str] == YES)
        return YES;
    else {
        return NO;
    }

}
```

#### 拨打电话

```objc
NSString * number = @"电话号码";        
NSMutableString * str=[[NSMutableString alloc] initWithFormat:@"tel:%@",number];
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:str]];
```

#### 使用 Auto Layout 布局时要禁用 translatesAutoresizingMaskIntoConstraints

　　`autoresizing(自动调整尺寸)` 是一种由IB所使用的 Struts-and-spring式布局工具，也可以指程序代码里用到的一些相关标志，例如UIViewAutoresizingFlexibleWidth等。如果要通过这些手段来调整视图的自动调整行为，那么在定义约束规则的时候，就不应该再引用该视图了。

　　使用约束规则之前，应该先禁用视图中的有关属性，该属性会把涉及自动调整功能的掩码自行转换为约束规则。如果启用该属性，那么视图会通过与自动调整功能有关的掩码来参与到Auto Layout 系统里面，要是禁用它，开发者就需要自己添加约束规则。

　　这个与约束规则有关的属性叫做`translatesAutoresizingMaskIntoConstraints`，如果将其设为`NO`,我们就可以放心的向视图中添加自己的约束规则，而不必担心它与系统自动生成的规则想冲突。这一点非常重要。若是没有禁用该属性，就开始添加约束，则可能会引起冲突。由自动调整功能所生成的那些规则无法与开发者自己编写的这些规则和平相处。

　　因此在基于`自动调整功能的布局`和基于`约束规则的布局`之间进行抉择是编写代码时的一项重要任务。

### 常识备忘

* 1、`UISwith`的`Width`和`Height`是`{51, 31}`。
* 2、`frame`是以该视图的父坐标系为参照来定义的；而`bounds`是根据当前视图自己的坐标系来定义的。



### 图片备忘

#### iphone 各款机型参数

![iphone](http://7xvffo.com1.z0.glb.clouddn.com/iphone%E5%90%84%E7%A7%8D%E6%9C%BA%E5%9E%8B%E5%8F%82%E6%95%B0.png)

#### 沙盒存放路径

![](http://7xvffo.com1.z0.glb.clouddn.com/%E6%B2%99%E7%9B%92%E8%B7%AF%E5%BE%84.png)


#### App生命管理周期

![](http://7xvffo.com1.z0.glb.clouddn.com/UIApplicationDelegate.png)

