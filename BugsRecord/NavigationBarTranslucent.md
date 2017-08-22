### 导航栏不透明搞得鬼

* *导航栏默认是半透明的！*
* **对于同样的frame，导航栏不透明会导致UI控件向下偏移64个点！**

在ViewController中的View中添加一个红色的View

```objc
UIView * redView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 100, 100)];
redView.backgroundColor = [UIColor redColor];
[self.view addSubview:redView];
```
当我们设置导航栏为`不透明`时

```objc
self.navigationController.navigationBar.translucent = NO;
```
效果图如图所示，

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/navigationbar.png" width="760" height="568" alt="选项列表图"/>
</div>
