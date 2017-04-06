### UIProgressView 进度条

#### 1、实例化一个进度条

*高度固定，不可以添加事件，只做显示使用，比如下载*

```objc
UIProgressView *progressView = [[UIProgressView alloc] initWithFrame:CGRectMake(100, 50, 200, 50)];
```	

#### 2、设置进度， 0～1

```objc
progressView.progress = 0;
```
    
#### 3、设置左边的颜色

```objc
progressView.tintColor = [UIColor redColor];
```
    
#### 4、设置右边的颜色

```objc
progressView.trackTintColor = [UIColor orangeColor];
```
    
