## 条件分支语句

## 判断

### if 语句

```swift
let age = 19
if age > 18 {
	print("adult")
}
```

### if...else...

```swift
if age > 18 {
	// ...
} else {
	// ...
}
```

### if...else if...else...

```swift
var light = "red"
var action = ""

if light == "red" {
    action = "stop"
} else if light == "yellow" {
    action = "caution"
} else if light == "green" {
    action = "go"
} else {
    action = "invalid"
}
```

### switch...case...

```swift
switch light {
    case "red":
        action = "stop"
    case "yellow":
        action = "caution"
    case "green":
        action = "go"
    default:
        action = "invalid"
        break
}
```

## 循环

### for...in...

```swift
for number in 1...5 {
    print(number)
}
```

### while...

```swift
var i = 0
while i < 5 {
    print(i)
    i += 1
}
```

### repeat...while...

```swift
repeat {
    print(i)
    i -= 1
} while i > 0
```

## 样式匹配

采用 `case 匹配的值 = 要检查的对象` 的方式，如检查是否在原点：

```swift
let pt = (x: 0, y: 0)
if case (0, 0) = pt {
    print("origin")
}
```

### value binding

```swift
let pt = (x: 0, y: 6)
switch pt {
    case (let x, 0):
        print("The point (\(x), 0) is on x axis")
    case (0, let y):
        print("The point (0, \(y)) is on y axis")
    default:
        break
}
```

### 自动提取 optional 非空值

```swift
let skills: [String?] = ["Swift", nil, "Python", nil]
for case let skill? in skills {
    print(skill)
}
```

### 自动绑定类型转换

```swift
let someValues: [Any] = [1, 2.0, "three"]
for value in someValues {
    switch value {
    case let value as Int:
        print("Integer: \(value)")
    case let value as Double:
        print("Double: \(value)")
    case let value as String:
        print("String: \(value)")
    default:
        break
    }
}
```

### where

```swift
for i in 1...5 where i % 2 == 0 {
    print(i)
}
```

### 使用 `,` 号串联条件

if A, then B, then C

```swift
if A, B, C {
	// ...
}
```



