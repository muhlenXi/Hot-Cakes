```swift
let numbers = [1, 2, 3, 4, 5]
        
for number in numbers {
    print(number)
}
    
for (index, number) in numbers.enumerated() {
    print("\(index) --> \(number)")
}
    
let fruits = ["apple": "🍎", "banana": "🍌", "orange": "🍊"]
for (key, value) in fruits {
    print("key: \(key), value: \(value)")
}
```