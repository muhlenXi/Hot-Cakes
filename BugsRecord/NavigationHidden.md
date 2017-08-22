### 导航栏隐藏那些事儿

**对于导航栏隐藏，以下两行代码都可以做到，但是要注意区别！**

```objc
self.navigationController.navigationBarHidden = YES;
```
*这行代码的作用是让整个 `navigationController` 都隐藏！*

```objc
self.navigationController.navigationBar.hidden = YES;
```
*这行代码的作用是仅仅隐藏 `navigationController` 中的一个`属性`,这个属性是 `navigationBar`!*

**区别如下：**

当我们使用 `navigationController` `Push`一个新的 `ViewController` 时，在 `ViewController` 界面的左边，从左往右滑时，会 `Pop` 到上一个`ViewController`!

* 1、当我们使用**第一行代码**隐藏导航栏时，不影响这个功能，**仍然可以**通过手势返回到上一个ViewController中。
* 2、当我们使用**第二行代码**隐藏导航栏时，则**无法**通过手势返回到上一个ViewCOntroller中。
