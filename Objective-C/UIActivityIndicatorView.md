### UIActivityIndicatorView 加载指示器

#### 1、实例化一个加载指示器

```objc
UIActivityIndicatorView *indicatorView = [[UIActivityIndicatorView alloc]  initWithActivityIndicatorStyle:UIActivityIndicatorViewStyleWhiteLarge];
indicatorView.frame = CGRectMake(200, 200, 100, 100); 
```

#### 2、开启动画

```objc
[indicatorView startAnimating];
```
    
    
#### 3、开启动画延时操作

    /*
    延时调用self的stopAnimatingAction方法
    withObject:参数
    afterDelay:一秒为单位
    */
    
```objc
[self performSelector:@selector(stopAnimatingAction:) withObject:indicatorView afterDelay:3];
```
    
#### 3、不动画的时候也显示

```objc
indicatorView.hidesWhenStopped = NO;
```
    
#### 4、判断是否在动画

```objc
if (indicatorView.isAnimating)
{
   NSLog(@"在动画");
}
```
