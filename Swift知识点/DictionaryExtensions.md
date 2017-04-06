### Dictionary Extensions

#### 1、合并 dictionary

```swift
mutating func merge <S: Sequence>(other: S)
    where S.Iterator.Element == (key: Key,value: Value) {
        for (key,value) in  other {
            self[key] = value
        }
    }
```

#### 2、用 tuple 数组初始化 Dictionary

```swift
init <S: Sequence> (other: S)
        where S.Iterator.Element == (key: Key,value: Value) {
        self = [:]
        self.merge(other: other)
    }
```

    