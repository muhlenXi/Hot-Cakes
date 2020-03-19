# 如何扩大按钮点击范围

override point 方法

```swift
class ExtendButton: UIButton {
    override func point(inside point: CGPoint, with event: UIEvent?) -> Bool {
        // 要扩大的事件响应范围偏移
        let offset: CGFloat = -10
        let bounds = self.bounds.insetBy(dx: offset, dy: offset)
        return bounds.contains(point)
    }
}
```