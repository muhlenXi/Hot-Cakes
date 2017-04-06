### UIAlertView && UIActionSheet 知识点

### iOS 8 之前

#### 1、创建一个警告框

```objc
UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"提示" message:@"网络连接错误" delegate:self cancelButtonTitle:@"取消" 
otherButtonTitles:@"确定", nil];
    
[alertView show];	    //显示警告框
```

#### 2、创建一个用于输入信息的警告框

```objc
UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"请输入备注" message:nil delegate:self cancelButtonTitle:@"取消"otherButtonTitles:@"确定",nil];
alertView.alertViewStyle = UIAlertViewStylePlainTextInput;
[self.noteAlert show];
```

#### 3、一般需要实现的代理方法

*需要遵循`UIAlertViewDelegate`协议*

```objc
#pragma mark - AlertViewDelegate

//用来填入历史内容
- (void)willPresentAlertView:(UIAlertView *)alertView
{
	if (alertView == self.noteAlert) {
        UITextField *tf = [alertView textFieldAtIndex:0];
        tf.textAlignment = NSTextAlignmentLeft;
        tf.text = @"要填入的文字";  
    }
}

```

*不同的alertView可以通过tag值或者属性来区分！*

```objc
//处理按钮的点击事件
- (void)alertView:(UIAlertView*)alertView clickedButtonAtIndex:(NSInteger)buttonIndex
{
    
    if ([alertView isEqual:self.hostipalAlert] && (buttonIndex == 1)) {
       
       //一般用来处理确定按钮的响应

    }
}
```

#### 4、创建一个底部选项列表

```objc
UIActionSheet *actionSheet = [[UIActionSheet alloc] initWithTitle:@"分享"  delegate:self cancelButtonTitle:@"取消" destructiveButtonTitle:@"销毁"  otherButtonTitles:@“QQ分享", @"微信分享", @"微博分享", nil];

[actionSheet showInView:self.view];
```

#### 5、底部选项列表需要实现的代理方法

*需要遵循`UIActionSheetDelegate`协议*

```objc
#pragma mark - UIActionSheetDelegate
- (void)actionSheet:(UIActionSheet *)actionSheet clickedButtonAtIndex:(NSInteger)buttonIndex
{
    if (actionSheet.tag == 6000) {
        if (buttonIndex == 0) {
            [self chooseImageFromLibary];
        }
        else if (buttonIndex == 1) {
            [self chooseImageFromCamera];
        }
    }
}
```

### iOS 8 之后

#### 6、创建警告框 or 底部选项列表

*一个警告框*

```objc

UIAlertController * alertController = [UIAlertController  alertControllerWithTitle:@"温馨提示" message:@"话费不足，请及时充值！"  preferredStyle:UIAlertControllerStyleAlert];
    
//创建取消按钮
UIAlertAction * action1 = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action)
{  
    //点击了按钮才会执行的block
    NSLog(@"取消按钮 点击了");
}];
    
UIAlertAction * action2 = [UIAlertAction actionWithTitle:@"销毁" style:UIAlertActionStyleDestructive handler:^(UIAlertAction * _Nonnull action)
{
     NSLog(@"销毁按钮 点击了");
}];
    
[alertController addAction:action1];	 //添加按钮
[alertController addAction:action2];
    
//显示 UIAlertController 
[self presentViewController:alertController animated:YES completion:nil];
```   


	
        

        

