### UILabel相关知识点

#### 1、初始化UILabel并设置大小

```objc
UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(50, 50, 200, 50)];
```

#### 2、显示文字

```objc
label.text = @"hello world";
```

#### 3、文字对齐方式

```objc
label.textAlignment = NSTextAlignmentCenter;   //居中对齐
```

#### 4、设置字体

```objc
// 正常
UIFont *font = [UIFont systemFontOfSize:20];
// 加粗          
UIFont *font1 = [UIFont boldSystemFontOfSize:20]; 
// 斜体    
UIFont *font2 = [UIFont italicSystemFontOfSize:25];  
// 自定义字体样式
NSArray *array = [UIFont familyNames];
NSLog(@"字体族：%@", array); 
UIFont *font3 = [UIFont fontWithName:@"Bodoni 72 Smallcaps" size:20];
    
// 设置字体(默认是系统的17号字体)
label.font = font3;
```

#### 5、设置字体颜色

```objc
label.textColor = [UIColor whiteColor];
```

#### 6、打开高亮状态并设置高亮颜色

```objc
label.highlighted = YES;
label.highlightedTextColor = [UIColor redColor];
```

#### 7、设置多行显示

```objc
label1.numberOfLines = 0;
```

#### 8、设置换行形式

```objc
//以单词换行
label1.lineBreakMode = NSLineBreakByWordWrapping；  
```

#### 9、缩放字体，让label可以完整显示

```objc
label1.adjustsFontSizeToFitWidth = YES;
```

#### 10、设置阴影颜色 和 阴影偏移

```objc
label2.shadowColor = [UIColor redColor];
label2.shadowOffset = CGSizeMake(10, 10);
```

#### 11、计算label的高度
    
boundingRectWithSize:label最大的size

options:以原始的字体样式来计算

attributes:字体
     
```objc
CGRect rect = [label.text 	boundingRectWithSize:CGSizeMake(label.frame.size.width, 2000) options:NSStringDrawingUsesLineFragmentOrigin 		attributes:@{NSFontAttributeName : label.font} context:nil];
NSLog(@"rect : %@", NSStringFromCGRect(rect));
```

#### 12、frame,center,bounds结构体改变，需要整个结构体改变
    
```objc
CGRect frame = label.frame;	
//向上取整方法
frame.size.height = ceil(rect.size.height)
label.frame = frame;
```

#### 13、设置 边框宽度 和 边框颜色 

```objc
label.layer.borderWidth = 2;
label.layer.borderColor = [UIColor yellowColor].CGColor;
```
#### 14、设置倒角 && 切割超过边缘的部分

```objc
label.layer.cornerRadius = 10;           
label.clipsToBounds = YES;
```

#### 15、增加UILabel的视差效果

```objc
UIInterpolatingMotionEffect * motionEffect = [[UIInterpolatingMotionEffect alloc] 	initWithKeyPath:@"center.x" type:UIInterpolatingMotionEffectTypeTiltAlongHorizontalAxis];
motionEffect.minimumRelativeValue = @(-25);
motionEffect.maximumRelativeValue = @(25);
[msgLabel addMotionEffect:motionEffect];
motionEffect = [[UIInterpolatingMotionEffect alloc] initWithKeyPath:@"center.y" type:UIInterpolatingMotionEffectTypeTiltAlongVerticalAxis];
motionEffect.minimumRelativeValue = @(-25);
motionEffect.maximumRelativeValue = @(25);
[msgLabel addMotionEffect:motionEffect];
```

#### 16、根据文字调整UILabel的大小

```objc
[msgLabel sizeToFit];
```

