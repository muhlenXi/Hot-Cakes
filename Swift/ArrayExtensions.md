### Array Extensions

#### 1、获取 Array 的地址

```swift
func getBufferAddress<T>(of array: [T]) -> String {
    return array.withUnsafeBufferPointer {
        buffer in
        return String(describing: buffer.baseAddress)
    }
}
```

#### 2、Map 的实现

```swift
func myMap<T> (_ transform:(Element) -> T) -> [T] {
    var tmp: [T] = []
    tmp.reserveCapacity(count)
    for value in self {
        tmp.append(transform(value))
    }
    return  tmp
}
```
#### 3、类似 reduce ，不过将所有元素合并到一个数组中，而且保留合并时每一步的值

```swift
func  accumulate<T> (_ initial: T,
                     _ Sum:(T,Element) -> T) -> [T] {
    var  currentSum = initial
    return  map { item in
        currentSum = Sum(currentSum,item)  // Change inter sum value
        return currentSum
    }
}
```

#### 4、Filter 的实现

```swift
func myFilter(_ includeElement: (Element) -> Bool) -> [Element] {
    var  result: [Element] = []
    for item in self  where includeElement(item){
        result.append(item)
    }
    return  result
}
```
#### 5、判断数组中的所有元素是否都满足条件

```swift
// 对于一个条件，如果没有元素不满足它的话，那意味着所有元素都满足它。
func  allMatch(_ predicate: (Element) -> Bool) -> Bool {
    return !contains { !predicate($0)}
}
```

#### 6、Reduce 的实现

```swift
func myReduce<T> (_ initial: T, combine: (T,Element) -> T) -> T {
    var result = initial
    for item in self {
        result = combine(result,item)
    }
    return  result
}
```
    