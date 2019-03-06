### Swift 数据结构之 Set

### 基本概念

> Set 是包含唯一元素的无序集合。


#### 使用场景：

- 不考虑集合中元素排列顺序

- 确保集合中的元素是唯一的
- 集合运算

#### Set 的基础操作

你可以创建包含任意元素的 Set，只要该元素遵循  `Hashable` 协议。Swift 标准库中的元素默认的都遵循 Hashable 协议，包括 String，数值类型，Bool ，无关联类型的枚举，以及 Set 本身。

- 可以通过类似创建  Array  的方式创建  Set

- 可以通过  `contains `  判断 Set 是否包含指定元素

- 可以通过  `==`  判断两个  Set  是否相等

- 可以通过  `isSubset `  判断一个 Set 是否是另一个 Set 的子集

- 可以通过  ` isStrictSuperset`  判断一个 Set 是否是另一个 Set 的严格子集

- 可以通过  `isSuperset  ` 判断一个 Set 是否是另一个 Set 的超集

- 可以通过  `isStrictSuperset  ` 判断一个 Set 是否是另一个 Set 的严格超集

- 可以通过  `isDisjoint `  判断两个 Set 是非相交的（即是否不包含相同元素）

  

  *以下是示例代码，自己动手在 playground 中敲一遍会印象深刻的，编程之路没有捷径。*

  

  ```swift
  let fruits: Set = ["apple", "banana", "grape"]
  let fruits1: Set = ["grape", "banana"]
  let fruits2: Set = ["watermelon"]
  
  fruits.contains("apple")
  fruits.contains("mango")
  
  fruits == fruits1
  
  fruits1.isSubset(of: fruits)
  fruits.isSuperset(of: fruits1)
  
  fruits1.isStrictSubset(of: fruits1)
  fruits1.isSubset(of: fruits1)
  
  fruits1.isStrictSuperset(of: fruits1)
  fruits1.isSuperset(of: fruits1)
  
  
  fruits.isDisjoint(with: fruits1)
  fruits.isDisjoint(with: fruits2)
  ```

  

  #### Set 与 Set 之间的操作

  

  - 可以通过 `union `  获取两个 Set 的并集
  - 可以通过 `intersection ` 获取两个 Set 的交集
  - 可以通过 `subtracting `  获取两个 Set 的差集
  - 可以通过 `symmetricDifference `  获取两个 Set 的对称差集( 并集 - 交集)

  

  示例如下：

  

  ```swift
  let fruits3 = fruits1.union(fruits2)
  let intersection = fruits3.intersection(fruits2)
  
  let fruits4: Set = ["apple", "banana"]
  let fruits5: Set = ["banana", "watermelon"]
  let fruits6 = fruits4.symmetricDifference(fruits5)
  
  let fruits7 = fruits3.subtracting(fruits1)
  let fruits8 = fruits4.subtracting(fruits5)
  ```

  

  #### Set 的集合操作

  

  对于集合的操作， Set 也有。

  - 通过 `isEmpty ` 判断 Set 是否为空
  - 通过 `for-in` 循环遍历 Set 中的元素
  - 执行 `map` 、 `reduce`、 `filter` 等操作。

  

  #### Set 和 NSSet 的转换

  

  通过 `as`  操作符可以在 Set 和 NSSet 之间转换，需满足如下条件：

  - Set 中的元素类型是 Foundation 类型。
  - Set 中的元素是遵循 ` @objc ` 协议的类型。

  

  从 Set 转换成 NSSet 的时间复杂度是 o(1)。当 Set 中的元素类型不是遵循 ` @objc ` 协议的类型时，在首次访问其中每个元素的时候会进行转换。所以对 Set 的操作的时间复杂度是 o(n)。

  

  从 NSSet 转换 Set 时，会首先调用  ` copy(with:)`  方法获取一份不可变拷贝，然后执行时间复杂度为 o(1)  的  Swift bookkeeping 操作。NSSet 的实例一直是不可变的，  ` copy(with:)`  方法会返回相同实例。NSSet 和 Set 的实例都使用 copy-on-write 机制，也就是说两个 Set 是共享存储的。

  

  #### 后记

  [本文的 demo](https://github.com/muhlenXi-Team/Swift-data-structure) 在这里，下载后运行可体验本文所描述的效果。*关于 Set 的更多详细信息请访问 [Set](https://developer.apple.com/documentation/swift/set) 。

  

  *哇，读到这里了，真棒！每天进步一点点！为你的专注点赞！我们下篇见*

  

  







