### Swift内存管理

#### 类引用循环的处理方式

* 一方属性可以为 nil 时，使用 *weak* 关键字修饰。
* 一方属性不可以为 nil 时，使用 *unowned* 关键字修饰
* 双方属性都不可以为 nil 时，用 *implicit unwrapped optional* 把其中一个属性伪装起来，让它还是一个 optional，另一个属性定义为 *unowned* 就好了。

#### closure 引用循环的处理方式

* 使用 *unowned* 处理 closure 和类对象同生共死的引用循环。
* 使用 *weak* 处理 closure 和类对象生命周期不同的引用循环。

**用好 Closure 的关键三点：**

* 确定类对象是否真正拥有一个 closure；
* 确保自己理解 closure 按引用捕获对象的含义；
* 理解 capture list 的行为；