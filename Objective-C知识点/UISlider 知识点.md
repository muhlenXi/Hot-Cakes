### UISlider 知识点

#### 1、实例化一个滑块

```objc
UISlider *slider = [[UISlider alloc] initWithFrame:CGRectMake(100, 50, 300, 50)];
```
    
#### 2、设置滑块的值，默认为 0 ～ 1

```objc
slider.value = 0.5;
```
    
#### 3、设置左右两边的颜色

```objc
slider.minimumTrackTintColor = [UIColor redColor];
slider.maximumTrackTintColor = [UIColor greenColor];
```
    
#### 4、设置左右两边的图片

```objc
slider.minimumValueImage = [UIImage imageNamed:@"AudioPlayerScrubberKnob"];
slider.maximumValueImage = [UIImage imageNamed:@"AudioPlayerVolumeKnob"];
```
    
#### 5、设置滑块按钮的颜色

```objc
slider.thumbTintColor = [UIColor blueColor];
```
    
#### 6、设置滑块按钮的图片

```objc
[slider setThumbImage:[UIImage imageNamed:@"AudioPlayerScrubberKnob"] forState:UIControlStateNormal];
			
[slider setThumbImage:[UIImage imageNamed:@"AudioPlayerVolumeKnob"] forState:UIControlStateHighlighted];
```    
#### 7、改变value的范围200~1000

```objc
slider.minimumValue = 200; 
slider.maximumValue = 1000;
```
    
#### 8、添加事件

```objc
[slider addTarget:self action:@selector(sliderAcion:) forControlEvents:UIControlEventValueChanged];
```

