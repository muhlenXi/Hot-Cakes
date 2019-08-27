如何禁止单个 ViewController 的视图自动旋转？

需要分别对 UINavigationController、UITabBarController 和你所在的 ViewController 的 shouldAutorotate 属性进行 override。

**第一步：UINavigationController extension**

```swift
extension UINavigationController {
    open override var shouldAutorotate: Bool {
        return visibleViewController?.shouldAutorotate ?? true
    }
}
```

**第二步：UITabBarController extension**

```swift
extension UITabBarController {
    open override var shouldAutorotate: Bool {
        return selectedViewController?.shouldAutorotate ?? true
    }
}
```

**第三步：UIViewController**

```swift
class NewsBaseViewController: UIViewController {

    override var shouldAutorotate: Bool {
        return false
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

}

```