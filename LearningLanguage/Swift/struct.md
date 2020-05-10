```swift
struct Rectangle {
    private(set) var width: Double
    private(set) var length: Double
    
    var area: Double {
        return length * width
    }
    
    var circumference: Double {
        return (length + width) * 2
    }
}
```