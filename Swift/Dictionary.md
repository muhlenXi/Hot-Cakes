## Dictionary

Dictionary 也是值语义的。


### 创建

```swift
let dic: [KeyType: ValueType] = [key: value]
```

### 基本属性

```swift
dic.count
dic.isEmpty

dic.keys
dic.values
```

### 添加、更新、删除

```swift
dic["name"] = "muhlenXi"

// 可以获得修改前的值
dic.updateValue("Mars", forKey: "name")

// 删除
dic["name"] = nil
```

### 遍历

```swift
for (k, v) in dict {
	// ...
}
```

### Merge

*无论任何形式的序列，只要它的元素中 key 和 value 的类型和 Dictionary 相同，就可以进行合并。*

```swift
extension Dictionary {
    mutating func merge<S: Sequence>(_ sequence: S)  where S.Iterator.Element == (key: Key, value: Value) 	{
        
        sequence.forEach {
            self[$0] = $1
        }
        
    }
}

```

### Via tuple array init Dictionary

```swift
extension Dictionary {
    mutating func init<S: Sequence>(_ sequence: S)  where S.Iterator.Element == (key: Key, value: Value) 	{
        
      self = [:]
      self.merge(sequence)   
    }
}
```






