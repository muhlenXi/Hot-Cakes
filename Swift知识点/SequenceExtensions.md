### Sequence Extensions

#### 1、获取序列中所有的唯一元素

```swift
func unique() -> [Iterator.Element] {
        var result: Set<Iterator.Element> = []
        return filter {
            if result.contains($0){
                return false
            } else  {
                result.insert($0)
                return true
            }
        }
    }
```



    