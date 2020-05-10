```swift
let numbers = Set(arrayLiteral: 1, 2, 3, 1, 2)
let numbers1 = Set([3, 4, 5, 6])
    
let intersection = numbers.intersection(numbers1)
let union = numbers.union(numbers1)
let subtraction = numbers.subtracting(numbers1)
```