### UIImage 使用

1、防止图片拉伸变形

```swift
let image = UIImage(named: "hello").stretchableImage(withLeftCapWidth: 2, topCapHeight: 0)
```

2、防止图片被渲染成蓝色

```swift
let image = UIImage(named: "hello").withRenderingMode(.alwaysOriginal))
```