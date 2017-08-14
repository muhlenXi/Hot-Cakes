### UIGestureRecognizer 知识点

#### 1、添加点击手势（tap）

*initWithTarget:手势的响应者*

*action:响应者的方法*

```objc
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] 
			initWithTarget:self action:@selector(tapAction:)];
```

*打开用户交互 && 添加手势*

```objc
self.view.userInteractionEnabled = YES;
[self.view addGestureRecognizer:tap];
```

*手势响应方法*

```objc
- (void)tapAction:(UITapGestureRecognizer *)tap
{
	UIView * view = (UIView *) tap.view;
}
```

#### 2、添加捏合手势（pinch）

```objc
//添加捏合手势
        let pinGesture = UIPinchGestureRecognizer.init(target: faceView, action: #selector(faceView.pinchScale(gesture:)))
        faceView.addGestureRecognizer(pinGesture)
```

*捏合手势响应*

```objc
func pinchScale(gesture: UIPinchGestureRecognizer) {
        
    if gesture.state == .changed {
         
         //处理手势响应   
         scale *= gesture.scale   
         gesture.scale = 1   //注意每次置1
    }
}


```