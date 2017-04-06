### Swift代理

### 被代理者（委托人）需要做的事情

#### 1、协议的声明

```objc
//声明一个数据源协议
protocol HappynessViewDatasource: class {
    
    func smilinessForHappynessView(sender: HappynessView) -> Double?
}
```

#### 2、代理的声明

```objc
//数据源代理
weak var dataSource: HappynessViewDatasource?
```

#### 3、代理调用代理方法

```objc
//数据源代理调用
let smileness = dataSource?.smilinessForHappynessView(sender: self) ?? 0.0
```

### 代理人需要做的事情

#### 1、遵循代理协议

```objc
class HappynessViewController: UIViewController,HappynessViewDatasource {
	//自定义ViewController
}
```

#### 2、设置代理为self

```objc
@IBOutlet weak var faceView: HappynessView! {
    didSet {
        faceView.dataSource = self
    }
}
```

#### 3、实现协议中的代理方法

```objc
func smilinessForHappynessView(sender: HappynessView) -> Double? {
        
    return Double(happiness-50)/50
}
```
