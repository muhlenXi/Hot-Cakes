## Set

Set，是 Swift 标准库中的另一个主要的无序集合类型。

### 创建

```swift
var vowel: Set<Character> = ["a", "e", "i", "o", "u"]
```

### 常用属性和方法

```swift
vowel.count
vowel.isEmpty

vowel.contains("o")
vowel.remove("a")
vowel.insert("a")
vowel.removeAll()

vowel.sorted()
```

### 遍历

```swift
fot character in vowel {
	// ...
}

vowel.forEach { // ... }
```

### 代数运算

```swift
var setA: Set = [1, 2, 3, 4, 5, 6]
var setB: Set = [4, 5, 6, 7, 8, 9]

// 交集
let intersectionAB = setA.intersection(setB)

// 并集 - 交集
let symmetricDiffAB = setA.symmetricDifference(setB)

// 并集
let unionAB = setA.union(setB)

// 差集
let aSubtractB = setA.subtracting(sestB)
```

*注意：API 名称前有 from ，指的是可修改 Set 自身。*

如： setA.fromIntersection(setB)

### 其他用途

把 Set 用作内部支持类型

IndexSet

CharacterSet






