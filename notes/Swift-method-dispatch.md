Swift 方法派发机制

函数派发的目的是告诉 CPU 被调用的函数在哪里。

### 基本函数派发方式

编译型语言有三种基本函数派发方式

- Direct Dispatch 直接派发，又称静态派发
- Table Dispatch 函数表派发
- Message Dispatch 消息派发

直接派发速度快，可以被编译器优化。局限是缺乏动态性，所以没办法支持继承。

函数表派发是编译语言常见的实现方式。函数表使用数组来存储类中声明的每个函数的指针。这个表，大部分语言叫 Virtual table，Swift 中叫 Witness table。

消息机制派发是最动态的方式，开发者可以在运行时改变函数的行为。

### Swift 函数派发方式

Swift 中的函数派发方式与以下四种因素相关：

- 声明的位置
- 引用类型
- 指定行为
- Swift 显示优化

Swift 的函数可以声明两个地方，一个是类型声明作用域，二是 extension 中。通过下表可以看出声明位置带来的差异。

| 类型  |  类型声明作用域 |  extension |
| :---:| :---:|:---:|
| Value Type | Static | Static |
| Protocol | Table | Static |
| Class | Table | Static |
| NSObject Class | Table |  Message |

引用类型不同也会导致派发方式不同。

Swift 的一些修饰符可以指定派发方式。

- final 允许类里面的函数使用直接派发。
- dynamic 可以让类里面的函数使用消息机制派发。
- @objc 和 @nonobjc 显示声明一个函数能否被 Objective-C 的运行时捕获到。
- @inline 告诉编译器可以使用直接派发。

Swift 会尽最大能力优化函数派发方式。比如你有一个函数从来没有 override， Swift 则会尽可能使用直接派发的方式。

参考资料
-  [Method Dispatch in Swift](https://www.rightpoint.com/rplabs/switch-method-dispatch-table)

