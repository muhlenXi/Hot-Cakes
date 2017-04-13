### UITextField相关知识点  
	
#### 1、创建一个文本输入框

```objc
UITextField *textField = [[UITextField alloc]
			initWithFrame:CGRectMake(100, 100, 200, 50)];
```

#### 2、设置边框样式

*UITextBorderStyleNone, 无样式*

*UITextBorderStyleLine, 黑色边框*

*UITextBorderStyleBezel, 灰色边框*

*UITextBorderStyleRoundedRect 圆角边框*

```objc
textField.borderStyle = UITextBorderStyleNone;
```	

#### 3、设置字体

```objc
textField.font = [UIFont systemFontOfSize:25];
// 当文本显示不完时，可以调整字体大小
textField.adjustsFontSizeToFitWidth = YES;
// 设置显示的最小字体
textField.minimumFontSize = 20;
```

#### 4、设置字体对齐方式

```objc
textField.textAlignment = NSTextAlignmentLeft;
```

#### 5、设置键盘 类型 样式

```objc
textField.keyboardType = UIKeyboardTypeASCIICapable;
// 键盘的样式	
textField.keyboardAppearance = UIKeyboardAppearanceLight;	
```

#### 6、设置提示文字

```objc
textField.placeholder = @"用户名/手机号/邮箱";
```

#### 7、设置密文输入

```objc
textField.secureTextEntry = YES;
```

#### 8、设置文本内容

```objc
textField.text = @“welcome”;
```

#### 9、设置清理模式

*UITextFieldViewModeNever, 都不显示*

*UITextFieldViewModeWhileEditing, 编辑时显示*

*UITextFieldViewModeUnlessEditing, 不编辑的时候显示*

*UITextFieldViewModeAlways 都显示*

```objc
textField.clearButtonMode = UITextFieldViewModeAlways;
```

#### 10、设置return按钮的样式

```objc
textField.returnKeyType = UIReturnKeyJoin;
```

#### 11、收起键盘
		
```objc
// 第一种方式
[self.view endEditing:YES];//让self.view或其子视图结束编辑		
// 第二种方式,放弃第一响应者身份
[textField resignFirstResponder];			
```   

#### 12、成为第一响应者

```objc
// 成为第一响应者
[textField becomeFirstResponder];	
```

#### 13、获取消息中心  
		
*消息中心，广播中心,通知中心 NSNotificationCenter (单例)*

```objc			
NSNotificationCenter *center = [NSNotificationCenter defaultCenter];
```

#### 14、注册通知成为观察者

*addObserver:监听者*

*selector:当监听到事件时出触发的方法*

*name:监听的消息名字，字符串类型*

*object:用户过滤消息，一般为nil*

```objc
[center addObserver:self selector:@selector(nofiticationAction) 	
			name:@"MyNotification" object:nil];
```

#### 14、发送消息(通知)

*postNotificationName:消息名字*

*object:发送消息时，传的参数*

```objc
[center postNotificationName:@"MyNotification" object:@"test"];
```

#### 15、移除监听

```objc
[center removeObserver:self name:@"MyNotification" object:nil];
```

#### 16、监听键盘的消息

*UIKeyboardWillShowNotification  键盘将要出来*
*UIKeyboardWillHideNotification  键盘将要隐藏*

```objc
NSNotificationCenter *center = [NSNotificationCenter defaultCenter];

// 监听键盘将要弹出的消息
[center addObserver:self selector:@selector(keyboardWillShow:) 
		name:UIKeyboardWillShowNotification object:nil];
		
[center addObserver:self selector:@selector(keyboardWillHide:) 
		name:UIKeyboardWillHideNotification object:nil];
```

#### 17、用Done键来关闭文本框的键盘

```objc
//设置返回键类型
[self.textField setReturnKeyType:UIReturnKeyDone];
//设置清除按钮  只在编辑时出现
[self.textField setClearButtonMode:UITextFieldViewModeWhileEditing];
```

*需要遵循`UITextFieldDelegate`协议,实现如下代理方法：*

```objc
#pragma mark - UITextFieldDelegate
- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    [textField resignFirstResponder];
    return YES;
}
```
  

#### 18、监听 输入变化

```objc
[self.textField addTarget:self
                   action:@selector(textFieldDidChange:)
         forControlEvents:UIControlEventEditingChanged]; // 监听事件

// 时间处理         
- (void) textFieldDidChange:(UITextField *) sender {
    
    // 文本内容
   NSLog(@"text == %@",sender.text);
    
}
```         

#### 19、UITextFieldDelegate 协议

     1）是否可以进入编辑模式
     	- (BOOL)textFieldShouldBeginEditing:(UITextField *)textField;

     2）文本框已经进入编辑模式
     	-(void)textFieldDidBeginEditing:(UITextField *)textField;

     3）文本框是否可以结束编辑模式
     	-(BOOL)textFieldShowEndEditing:(UITextField *)textField;


     4）文本框已结束编辑模式
     	-(void)textFieldDidEndEditing:(UITextField *)textField;

     5）是否可以点击clear按钮
     	-(BOOL)textFieldShouldClear:(UITextField *)textField;

     6）是否可以点击return按钮
     	-(BOOL)textFieldShouleReturn:(UITextField *)textField;
  
     7）允许修改内容
     	- (BOOL)textField:(UITextField *)textField 
		shouldChangeCharactersInRange:(NSRange)range 
		    replacementString:(NSString *)string;
    

    

