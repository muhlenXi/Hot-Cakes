---
title: objc 与 objcMembers 的区别是什么？
date: 2018-08-08 18:08:37
tags:
---

*版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: <http://muhlenxi.com/2018/08/08/objcmembers>*

> 在 Swift 与 Objective-C 混编的项目中一般会用到 @objc 和 @objcMembers 这两个关键字。那它们有什么不同呢？

<!-- more -->

### 1 - Objective-C 访问 Swift

在 Swift4 中，如果我们想要在 Objective-C 中访问 Swift 文件中声明的属性和方法，我们声明的类只需要继承 NSObject 基类 即可。这种情况下，在 Objective-C 中无法访问该类的属性和调用该类的方法，也就是说在编译成生成的 **项目名-Swift.h ** 文件中找不到该类的桥接属性和方法，自然你也无法使用。

- 如果你只想暴露**个别的**属性和方法给 Objective-C 访问和调用，你只需要在要暴露的属性和方法前添加 **@objc** 关键字即可。

举例如下：

```swift
class Person: NSObject{
    /// 只暴露性别属性
    @objc var sex: Int = 0
    var name = "anoymous"
    
    func sayhello() {
        print("I like \(sex)")
    }
    
    @objc func sayByebye() {
        print("Good bye")
    }
}
```

我们看一下生成的 Objective-C 桥接文件：

```objc
SWIFT_CLASS("_TtC11ExAttribute6Person")
@interface Person : NSObject
/// 只暴露性别属性
@property (nonatomic) NSInteger sex;
- (void)sayByebye;
- (nonnull instancetype)init OBJC_DESIGNATED_INITIALIZER;
@end
```



- 如果你想暴露该类的**全部**方法和属性给 Objective-C 访问和调用，你则需要在声明类的时候添加 **@objcMembers** 关键字即可。

举例如下：

```swift
@objcMembers class Boy: NSObject {
    var name: String = ""
    var age = 0
    
    func sayHello() {
        print("Hello, I am \(name). I am \(age) yearsold.")
    }
}
```

同样我们也看一下生成的 Objective-C 桥接文件：

```objc
SWIFT_CLASS("_TtC11ExAttribute3Boy")
@interface Boy : NSObject
@property (nonatomic, copy) NSString * _Nonnull name;
@property (nonatomic) NSInteger age;
- (void)sayHello;
- (nonnull instancetype)init OBJC_DESIGNATED_INITIALIZER;
@end
```

通过以上示例，我们可以得出结论，@objc 与 @objcMembers 的**关键区别**在于暴露给 Objective-C 的属性、方法的范围。如果我们想要保护业务实现的细节，就需要合理的使用 @objc 与 @objcMembers 关键字。

### 2 - Swift 访问 Objective-C

如果我们想要访问 Objective-C 文件中声明的类、属性和方法，我们只需要在 **项目名-Bridging-Header.h** 桥接头文件中导入我们想访问的类即可。

举例如下：

```objc
#import "OCViewController.h"
```

如需要本文完整示例 demo，请访问 [ExploreAttribute](https://github.com/muhlenXi-Team/ExploreAttribute)。

### 后记

写代码和做学问类似，要时刻保持严谨的态度。对于关键的知识点一定要理解透彻，不能懵懵懂懂，这样写出的代码才会可靠。

编码寄语：编程是一种技艺，一种需要用心学习的技艺！

