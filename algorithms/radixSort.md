## 算法实现

用 Swift 实现的算法如下:

```swift
func radixSort(unsortedArray: [Int]) -> [Int]{
    var unsortedArray = unsortedArray
    
    var tempArray = [Int]()
    var maxValue = 0
    var maxDigit = 0
    var level = 0
    
    repeat {
        var buckets = [[Int]]()
        for _ in 0..<10 {
            buckets.append([Int]())
        }
        
        for i in 0..<unsortedArray.count {
            let elementValue = unsortedArray[i]
            
            let divident = level < 1 ? 1 : NSDecimalNumber(decimal: pow(10, level)).intValue
            let mod = elementValue / divident % 10
            buckets[mod].append(elementValue)
            
            if maxDigit == 0 {
                if elementValue > maxValue {
                    maxValue = elementValue
                }
            }
        }
        
        tempArray.removeAll()
        for element in buckets {
            tempArray.append(contentsOf: element)
        }
        
        if maxDigit == 0 {
            while maxValue > 0 {
                maxValue = maxValue / 10
                maxDigit += 1
            }
        }
        
        unsortedArray = tempArray
        level += 1
        
    } while (level < maxDigit)
    
    return tempArray
}
```

用 Objective-C 实现的算法如下：

```objc
- (NSArray*)radixSort:(NSArray *) unsortedArray {
    NSMutableArray *tempArray;
    NSInteger maxValue = 0;
    NSInteger maxDigit = 0;
    NSInteger level = 0;
    
    do {
        // 1、创建10个桶
        NSMutableArray *buckets = [NSMutableArray array];
        for (int i = 0; i < 10; i++) {
            [buckets addObject:[NSMutableArray array]];
        }
        // 2、把数据保存到桶中
        for (int i = 0; i < unsortedArray.count; i++) {
            NSInteger elementValue = [unsortedArray[i] integerValue];
            
            int dividend = level < 1 ? 1 : pow(10, level);
            int mod = elementValue / dividend % 10;
            [buckets[mod] addObject:unsortedArray[i]];
            
            
            if (maxDigit == 0) {
                if (elementValue > maxValue) {
                    maxValue = elementValue;
                }
            }
        }
        // 3、合并桶中的数据
        tempArray = [NSMutableArray array];
        for (int i = 0; i < buckets.count; i++) {
            [tempArray addObjectsFromArray:buckets[i]];
        }
        
        if (maxDigit == 0) {
            while (maxValue > 0) {
                maxValue = maxValue / 10;
                maxDigit ++;
            }
        }
        
        // 4、继续下一轮排序
        unsortedArray = tempArray;
        level += 1;
        
    } while (level < maxDigit);
    
    return tempArray;
}
```