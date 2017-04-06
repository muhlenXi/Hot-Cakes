### UITouch 知识点

#### 1、如何捕捉触摸事件

*触摸开始*

```objc
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event;
```

*移动*

```objc
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event;
``` 

*触摸结束*

```objc
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;
```
      
*触摸被取消(触摸时候程序被中断)*

```objc
- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event;
```
#### 2、如何获取坐标信息

```objc
// 获取到触摸点
UITouch *touch=[touches anyObject]
// 转换为坐标点
[touch locationInView:self.view]
```
