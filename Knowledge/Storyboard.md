### Storyboard

#### 1、@IBDesignable 关键字可以让视图在storyboard中绘制出来。

```objc
@IBDesignable  
class HappynessView: UIView {
    //这是自定义View
}
```

#### 2、@IBInspectable  关键字可以让视图中的自定义属性在storyboard中可以被设置

*`didSet` && `willSet`为键值观察，每次重新设置值的时候，该方法会被调用。*

```objc
@IBInspectable
//线宽
var lineWidth:CGFloat = 3 {
    didSet {
        setNeedsDisplay()
    }
}
```
