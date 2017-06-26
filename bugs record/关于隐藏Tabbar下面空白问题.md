### 解决方法

2017-06-26 记录！

* 1、在第一个界面的 `viewWillAppear` 方法中添加 `self.tabBarController.tabBar.hidden = NO`;
* 2、在 *push* 前加上，`self.hidesBottomBarWhenPushed = YES`;
* 3、在第二个界面的 `viewWillAppear` 方法中添加 `self.tabBarController.tabBar.hidden = YES`; 
