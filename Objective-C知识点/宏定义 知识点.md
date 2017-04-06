### 宏定义 知识点

#### 1、屏幕宽和高的宏定义

```objc
#define kScreenWidth [UIScreen mainScreen].bounds.size.width
#define kScreenHeight [UIScreen mainScreen].bounds.size.height
```
#### 2、判断系统版本是否大于等于8.0 宏定义

```objc
#define GreaterThanEight ([[[UIDevice currentDevice] systemVersion] floatValue] > 8.0)
```

#### 3、判断是iphone 宏定义

```objc
#define IS_IPHONE (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone)
```

#### 4、判断是ipad

```objc
#define IS_IPAD (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad)
```

