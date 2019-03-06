---
title: Swift-Array
date: 2018-07-23 15:44:49
tags:
---



### Array

> 一种有序、随机访问的集合。



<!-- more -->

Array 是 app 开发中经常使用的数据类型。一般用 array 来组织 app 中的数据，比如保存多个相同类型。一个 array 可以存储任何类型的元素，从整型到字符串再到类。



#### 创建 Array

在 Swift  中创建 array 的方式比较简单，一般通过方括号包含用逗号分隔的多个元素，Swift 可以自动推断 array 中的元素类型。如：

```swift
// 一个包含 Int 类型的数组
let oddNumbers = [1, 3, 5, 7, 9, 11, 13, 15]

// 一个包含 String 类型的数组
let fruits = ["apple", "banana", "grape"]
```



也可以通过显示指定元素类型来创建空数组，如：

```swift
var emptyDoubles: [Double] = []

var emptyFloats: Array<Float> = Array()
```



如果需要为数组指定默认值，可以通过  `Array(repeating:count:)`  方法来创建，如：

```swift
var zeros = Array(repeatElement(0, count: 10))
```



#### 访问 Array 的元素

1、可以通过  `for-in`  循环来访问数组中的元素。

2、可以直接通过下标来访问数组的元素。第一个元素的下标永远是 0 ，一个包含 n 个元素的数组，它的元素的下标以为为 0 ~ n-1。

```swift
for fruit in fruits {
    print("I like eat \(fruit).")
}

fruits[0]
fruits[1]
```



- 用  `isEmpty`  来判断一个数组是否为空
- 用 `count` 获取数组包含的元素的个数
- 用 `first` 和 `last` 获取数组的第一个和最后一个元素



#### 增加和删除元素

- 通过 `append(_:)`  追加单个元素到数组末尾
- 通过 `append(contentsOf:)` 追加多个（其他数组中的）元素到数组末尾 
- 通过 `insert(_:at:)` 方法插入单个元素到数组指定位置
- 通过 `insert(contentsOf:at:)` 方法插入多个元素（其他数组的）到数组末尾
- 通过 `remove(at:)`  方法删除指定位置的元素
- 通过 `removeLast()` 方法删除数组末尾元素
- 通过 `removeSubrange(_:)`  方法删除数组指定区间的元素



```swift
var charates: [String] = []

charates.append("A")
charates.append(contentsOf: ["B", "C"])

charates.insert("D", at: 0)
charates.insert(contentsOf: ["E", "F"], at: 1)

charates.remove(at: 0)
charates.removeFirst()
charates.removeLast()

let range = 0 ... 1
charates.removeSubrange(range)
```



也可以通过下标来修改数组中的元素。如：

```swift
if let index = charates.index(of: "B") {
    charates[index] = "BBB"
}
```



#### 增加数组的大小

每个数组都持有特定大小的内存来保存内容。当你往数组中添加元素并超过他的预留容量时，数组会分配更大的内存区域，然后拷贝其中的元素到新区域。新存储一般是旧存储的几倍。这种指数增长策略的意义在于添加元素时会恒定时间重新分配内存，从而平均了许多添加操作的性能。添加操作触发重新分配内存会消耗性能，但是在数组变大的过程中，重新分配内存的操作不是很频繁的。

如果你知道要存储的元素个数，可以通过 `reserveCapacity(_:)` 方法在添加元素之前指定容量大小从而避免重新分配内存的情况。使用 capacity 和 count 属性来确定 array 能存储的元素数量而不是分配更大的内存空间。

对于元素类型是常见类型的数组，该存储是连续的内存块。如果元素类型是 类 或 遵循 @objc 协议的对象，该存储可能是连续的内存块。因为任何 NSArray 的子类都能变成 Array，所以在这种情况下无法表示和保证效率。



#### Array 拷贝元素的修改

每个数组都有一个包含元素值的独立值。对于简单类型比如 interger 和其他 struct，这意味着当你改变 array 中一个值时，该 array 副本中的值不会发生改变。如：

```swift
var numbers = [1, 2, 3, 4, 5]
var numbersCopy = numbers
numbers[0] = 100
print(numbers)
print(numbersCopy)
```



如果数组中的元素是类的实例时，则语义是相同的，尽管最初可能看起来不同。在这种情况下，array 中存储的值是对象的引用地址。如果你改变一个数组中对象的引用地址，只有该数组才有新对象的引用地址。如果两个数组中都包含同一个对象的引用地址，你能在两个数组中都看到对象属性的变化。如：

```swift
class IntegerReference {
    var value = 10
}
var firstIntegers = [IntegerReference(), IntegerReference()]
var secondIntegers = firstIntegers
firstIntegers[0].value = 100
print(secondIntegers[0].value)
// print 100

firstIntegers[0] = IntegerReference()
print(firstIntegers[0].value)
// print 10
print(secondIntegers[0].value)
// print 100
```



Array 像 Swift 标准库中能改变大小的集合一样，采用 copy-on-write 机制。array 的拷贝和 array 共享相同的存储空间，直到你改变其中的一个数组。





