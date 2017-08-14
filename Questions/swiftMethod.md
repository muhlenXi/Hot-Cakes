#### 3、如何理解 Swift 中的方法派发机制？

*Swift 的方法派发机制主要有3种，第一种是 Direct Dispatch，第二种是 Table Dispatch，第三种是 Message Dispatch。*

派发规则：

*1-值类型永远使用 Direct Dispatch。*

*2-在 protocol 和 class 的定义中声明的方法使用 table dispatch。*

*3-在 protocol 和 class 的 extension 中定义的方法使用 direct dispatch。*

修改方法派发规则的 modifiers:

*如果你的方法需要被 Objective-C 运行时识别，需要用 `dynamic` 将方法的派发方式改为 message dispatch，此外还必须 import Foundation，它包含了 Objective-C 运行时的核心组件。*

*`@objc`仅仅可以让一个 Swift 方法被 Objective-C 运行时识别和访问，但不会修改该方法的派发方式。*

*`final` 用于把方法的派发方式改为 direct dispatch。*

*`@nonobjc` 仅仅让一个方法对 Objective-C 运行时不可见，但不会修改方法的派发方式。*

[详情传送门](https://www.boxueio.com/series/understand-ref-types/ebook/187)