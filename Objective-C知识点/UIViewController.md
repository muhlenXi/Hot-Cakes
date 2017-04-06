### UIViewController相关知识点

#### 1、创建一个视图控制器

```objc
ViewControllerB *viewControllerB = [[ViewControllerB alloc] init];
```

#### 2、跳转（模态）到下一个控制器		

```objc
[self presentViewController:viewControllerB animated:YES completion:nil];
```

#### 3、设置模态动画

*UIModalTransitionStyleCoverVertical 从下往上的动画*

*UIModalTransitionStyleFlipHorizontal 旋转*

*UIModalTransitionStyleCrossDissolve, 渐变*

*UIModalTransitionStylePartialCurl 翻页*

```objc
viewControllerB.modalTransitionStyle = UIModalTransitionStylePartialCurl;
```

#### 4、回到上一级视图控制器

```objc
[self dismissViewControllerAnimated:YES completion:nil];
```


	
