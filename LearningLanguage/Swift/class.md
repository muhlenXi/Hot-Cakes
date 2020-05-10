```swift
class People {
    private var name: String
    private var age: Int
    private var gender: Gender
    
    init(name: String, age: Int, gender: Gender) {
        self.name = name
        self.age = age
        self.gender = gender
    }
}

enum Gender {
    case male
    case female
    case unknow
}
```