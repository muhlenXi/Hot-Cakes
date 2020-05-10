```swift
let characters = ["a": 96, "b": 97, "c": 98]
let counts = characters.count
    
var fruits = [String: Any]()
fruits["apple"] = "🍎"
fruits["banana"] = "🍌"
fruits["orange"] = "🍊"
    
fruits["apple"] = nil
fruits.removeValue(forKey: "apple")
fruits.removeAll()
```