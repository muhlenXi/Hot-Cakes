### UIView相关知识点

#### 1、CGRect转为字符串

```objc
NSLog(@"%@", NSStringFromCGRect(self.window.frame));
```

#### 2、初始化UIView并设置大小

```objc
UIView *view = [[UIView alloc] initWithFrame:CGRectMake(100, 100, 200, 200)];
```

#### 3、设置背景颜色

```objc
view.backgroundColor = [UIColor blueColor];
```

#### 4、设置view视图的中心点

```objc
view.center = CGPointMake(250, 250);
```

#### 5、设置view视图大小（不改变中心点）

```objc
view.bounds = CGRectMake(0, 0, 300, 300);
```

#### 6、获取 父视图

```objc
NSLog(@"redView的父视图: %@", redView.superview);
```

#### 7、获取 子视图

```objc
NSArray *subViewArray = view.subviews;
NSLog(@"subViews : %@", subViewArray);
```

#### 8、设置tag值  && 根据tag值获取view

```objc
greenView.tag = 1;
UIView *findView = [self.view viewWithTag:1];
NSLog(@"通过tag值找到的view ：%@", findView);
```

#### 9、删除视图(对自身发送从父视图中删除的消息)

```objc
[greenView removeFromSuperview];
```
#### 10、插入 视图

```objc
[view insertSubview:blueView atIndex:0];
//在指定的view之上
[view insertSubview:blueView aboveSubview:redView]; 
//在指定的view之下 
[view insertSubview:blueView belowSubview:redView];  
```

#### 11、交换视图顺序

```objc
[view exchangeSubviewAtIndex:0 withSubviewAtIndex:2];
```

#### 12、将视图放在 最前 || 最后

```objc
[view bringSubviewToFront:redView];	

[view sendSubviewToBack:redView];	
```

#### 13、判断是不是子视图

```objc
if ([redView isDescendantOfView:view]) {
   NSLog(@"YES redView 是 view 的子视图");
}
```
#### 14、动画设置 方式1

```objc
// 开始动画 (无先后顺序，都是同时执行)
[UIView beginAnimations:nil context:nil];
// 设置动画 参数是动画时间，以秒为单位
[UIView setAnimationDuration:5];
// 动画的内容	    	
CGRect frame = view.frame;			
frame.size.height = 200;
frame.size.width = 200;  
view.frame = frame;
// 提交动画
[UIView commitAnimations];	
```
#### 15、动画设置 方式2

```objc
[UIView animateWithDuration:2 animations: ^{
//动画内容
 CGRect frame = view.frame;			
 frame.size.width = 200;
 view.frame = frame;
        
 } completion:^(BOOL finished) {
    //完成后需要做的事件
	view.backgroundColor = [UIColor yellowColor];
}];
```
#### 16、动画嵌套

```objc
[UIView animateWithDuration:1 animations:^{

	//先改变frame
	CGRect frame = view.frame;	
	frame.size.width = 200;
	view.frame = frame;
} completion:^(BOOL finished) {

	[UIView animateWithDuration:2 animations: ^{
		//再改变颜色 
    	view.backgroundColor = [UIColor yellowColor];	  
    } completion:nil];
}];
```

#### 14、给UIView添加阴影

```objc
//添加阴影
self.layer.shadowColor = [[UIColor blackColor] CGColor];
//阴影的透明度
self.layer.shadowOpacity = 0.7f;
//阴影的偏移
self.layer.shadowOffset = CGSizeMake(0, 0);
```

#### 15、设置父视图的时候，只对父视图的透明度进行更改，而不影响它上面子视图的透明度。

```objc
view.backgroundColor = [[UIColor blackColor] colorWithAlphaComponent:0.5];
```
