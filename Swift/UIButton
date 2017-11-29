#### 修改 UIbutton 的 title 和 image 位置

Mark：文字在左，图片在右。

```swift
let choose = UIButton(frame: CGRect(x: 0, y: 0, width: 75, height: 30))
choose.setTitle("Hello World", for: .normal)

let imageWidth = choose.imageRect(forContentRect: choose.frame).width
let titleFont = choose.titleLabel?.font!
let title = choose.titleLabel?.text!
let titileWidth = title?.size(attributes: [NSFontAttributeName: titleFont!]).width
let spacing = CGFloat(5)
choose.titleEdgeInsets = UIEdgeInsetsMake(0, -imageWidth*2, 0, 0)
choose.imageEdgeInsets = UIEdgeInsetsMake(0, 0, 0, -titileWidth!*2-spacing)
```

#### 顺时针旋转 UIButton image 180度

```swift
func routateSelectJianghuBtnImageFrom0ToPi() {
    let animation = CABasicAnimation(keyPath: "transform.rotation")
    animation.fromValue = 0
    animation.toValue = CGFloat.pi
    animation.duration = 0.25
    animation.isRemovedOnCompletion = false
    animation.fillMode = kCAFillModeForwards
    self.choose.imageView?.layer.add(animation, forKey: "transform.rotation")
}
```
