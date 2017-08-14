### UIStepper 步控件

#### 1、实例化一个步控件

```objc
UIStepper *stepper = [[UIStepper alloc] initWithFrame:CGRectMake(100, 100, 200, 200)];
[self.view addSubview:stepper];
```

#### 2、设置最小值

```objc
stepper.minimumValue = 10;
```
    
#### 3、设置最大值

```objc
stepper.maximumValue = 20;
```
    
#### 4、设置步长

```objc
stepper.stepValue = 2;	//默认最小值为0；步长为1
```
    
#### 5、设置可以循环

```objc
stepper.wraps = YES;
```
    
#### 6、设置长按只触发一次事件

```objc
stepper.continuous = NO;
```
    
#### 7、设置长按只加一次

```objc
stepper.autorepeat = NO;
```
    
#### 8、添加事件

```objc
[stepper addTarget:self action:@selector(stepperAction:) forControlEvents:UIControlEventValueChanged];
```
