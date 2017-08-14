### UIButton相关知识点

#### 1、初始化button

```objc
UIButton *button = [UIButton buttonWithType:UIButtonTypeSystem];
```

#### 2、设置button位置和大小

```objc
button.frame = CGRectMake(100, 100, 100, 50);
```

#### 3、设置按钮文字

```objc
[button setTitle:@"按钮" forState:UIControlStateNormal];
```

#### 4、设置文字颜色

```objc
[customBtn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
```

#### 5、设置图片 不会被拉伸

```objc
[customBtn setImage:[UIImage imageNamed:@"detail@2x"]
			forState:UIControlStateNormal];
```

#### 6、设置背景图片  会被拉伸

```objc
[customBtn setBackgroundImage:[UIImage imageNamed:@"bg.jpg"]   forState:UIControlStateNormal]
```

#### 7、给按钮添加触发事件

```objc
// addTarget:响应者
// action:响应者的方法
// forControlEvents:事件类型

[customBtn addTarget:self action:@selector(btnAction:) 
			forControlEvents:UIControlEventTouchUpOutside];
```

