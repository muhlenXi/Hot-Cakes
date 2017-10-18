## Array

Array 表示一组有序（先后顺序）数据集合。


Array 重要思想：

* **通过 closure 来参数化对数组的操作行为。**

### 创建 Array

```swift
// 方式1
var array1: Array<Int> = Array<Int>()

// 方式2
var array2: [Int] = []

// 指定初始值
var threeInts = [Int](repeating: 3, count: 3)

var fiveInts = [1, 2, 3, 4, 5]
```

### Array 属性

```swift
// 数组中元素的个数
array1.count

// 数组是否为空
array1.isEmpty
```

### 数组元素访问

```swift
// 下标访问，不被推荐。
fiveInts[0]
```

### ArraySlice

```swift
// 数组切片
fiveInts[0...2]

// 数组切片创建数组
Array(fiveInts[0..<2])
```

### 遍历数组

```swift
for value in fiveInts {
	// ...
}

for (index, value) in fiveInts.enumerated() {
	// ...
}

fiveInts.forEach {
	print($0)
}
```

### 添加、删除元素

```swift
// 添加
array1.append(1)

array1 += [1, 2, 3]

array1.insert(5, at: 0)

// 删除
array1.remove(at: 5)

// 数组为空时，会导致运行时错误
array1.removeLast()

// 数组为空时，会返回 nil
array1.popLast()
```

### 其他用法

```Swift
// 获取某个元素的索引
array.index { $0 == 1 }

array1.first
array1.last

```

### Array 与 NSArray 的差异

* Array 按照值语义实现的，复制一个 Array 对象时，会拷贝整个 Array 的内容。
* 为了提高 Array 的性能，使用的是 copy on write 的方式。
* Array 是否能被修改，是通过 var 和 let 关键字决定的。


* NSArray 按照引用语义实现的。其中 NSArray（不可变数组）、NSMutableArray（可变数组）。
* 

### map、filter、reduce、flatMap


map 对数组中的元素逐个进行变形并生成新的数组。

filter 过滤出满足特定条件的元素。

reduce 合并数组中的所有内容。

flatMap 

* 第一步像 map 方法那样，对元素进行某种规则的转换。
* 第二步，执行 flatten 方法，将数组中的元素一一取出来，组成一个新数组。

### 最大值 和 最小值

前提 ：数组中的元素需要实现 Equatable protocol

```swift
array1.max()
array1.min()
```
