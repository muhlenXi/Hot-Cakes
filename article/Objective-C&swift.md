Objective-C 与 Swift 交互

假如我们创建一个 Hello 的项目。

Objective-C 的类暴露给 Swift，也就是用 Swift 创建 Objective-C 的实例，需要做以下操作

- 1、在 Hello-Bridging-Header.h 桥接文件中导入 Objective-C 中的类。

```objc
#import "OCFile.h"
```

Swift 的类暴露给 Objective-C，也就是用 Objective-C 创建 Swift 的实例，需要做以下操作

- 1、自定义的类需要继承 `NSObject` 并且用 `@objcMembers` 关键字修饰。

```swift
@objcMembers class Dog: NSObject {
    var name: String = ""
    
    func eat() {
        print("\(name) is eating 🦴🦴🦴.")
    }
}
```

- 2、自定义的枚举需要继承 `Int` 并且使用 `@objc` 关键字修饰。

```swift
@objc enum Gender: Int{
    case male
    case female
    case unknown
}
```

- 3、使用的时候需要导入 Hello-Swift.h 头文件。

```swift
#import "Hello-Swift.h"
```

### 参考来源

- [Migrating Your Objective-C Code to Swift](https://developer.apple.com/documentation/swift/migrating_your_objective-c_code_to_swift)
- [Importing Objective-C into Swift](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift)
- [Importing Swift into Objective-C](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_swift_into_objective-c)


