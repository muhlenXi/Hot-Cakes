Swift 中创建单例的两种方式：

方式一： 单例生成时不设置单例的变量

```swift
class Singleton {
    static let sharedInstance = Singleton()
    
    private init(){}
}
```

方式二：单例生成时设置单例变量

```swift
class Singleton {
    static let sharedInstance: Singleton = {
        let instance = Singleton()
        // setup code
        return instance
    }()
    
    private init(){}
}
```

### 参考来源

- [Managing a Shared Resource Using a Singleton](https://developer.apple.com/documentation/swift/cocoa_design_patterns/managing_a_shared_resource_using_a_singleton)
