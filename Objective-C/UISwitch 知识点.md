### UISwitch 知识点

#### 1、实例化一个开关

```objc
UISwitch *switchView = [[UISwitch alloc] initWithFrame:CGRectMake(100, 100, 0, 0)];
[self.view addSubview:switchView];
```

#### 2、设置开关状态

```objc
switchView.on = YES;
```

#### 3、设置打开的颜色

```objc
switchView.onTintColor = [UIColor redColor];
```
    
#### 4、关闭的颜色

```objc
switchView.tintColor = [UIColor greenColor];
```
    
#### 5、设置按钮的颜色

```objc
switchView.thumbTintColor = [UIColor blueColor];
```

#### 6、添加事件

```objc
[switchView addTarget:self action:@selector(switchAction:) forControlEvents:UIControlEventValueChanged];
```

