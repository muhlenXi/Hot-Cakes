### 算法实现

用 Swift 实现的算法如下：

```swift
func bucketSort(unsortedArray: [Int]) -> [Int]{
    if unsortedArray.count < 2 {
        return unsortedArray
    }
    // Find max and min value.
    var minValue = unsortedArray[0]
    var maxValue = unsortedArray[0]
    for element in unsortedArray {
        if element > maxValue {
            maxValue = element
        }
        if element < minValue {
            minValue = element
        }
    }
    
    // Add elelment to buckets
    let bucketCount = 3
    var buckets = [[Int]]()
    for _ in 0..<bucketCount {
        buckets.append([Int]())
    }
    let intervalValue = Double(maxValue - minValue+1)/Double(bucketCount)
    for element in unsortedArray {
        let bucketIndex = Int(Double(element-minValue)/intervalValue)
        insertValueToBucket(bucket: &buckets[bucketIndex], value: element)
    }
    
    // Create array from bucket.
    var sortedArray = [Int]()
    for element in buckets {
        sortedArray.append(contentsOf: element)
    }
    return sortedArray
}
    
func insertValueToBucket(bucket: inout [Int], value: Int) {
    bucket.append(value)
    var index = bucket.count-1;
    while index-1 >= 0 {
        if bucket[index] < bucket[index-1] {
            bucket.swapAt(index, index-1)
        }
        index -= 1
    }
}
```

用 Objective-C 实现的算法如下：

```objc
- (NSArray*)bucketSort:(NSArray*) unsorted {
    if (unsorted.count < 2) {
        return unsorted;
    }
    // Find max and min value.
    double minValue = [unsorted[0] doubleValue];
    double maxValue = [unsorted[0] integerValue];
    for(NSNumber *element in unsorted) {
        if([element integerValue] > maxValue) {
            maxValue = [element integerValue];
        }
        if([element integerValue] < minValue) {
            minValue = [element integerValue];
        }
    }
    // Create buckets array
    NSInteger bucketTotalCount = 3;
    NSMutableArray *buckets = [NSMutableArray array];
    for(NSInteger i = 0; i < bucketTotalCount; i++) {
        [buckets addObject:[NSMutableArray array]];
    }
    // Add unsorted element to buckets array
    double space = (maxValue - minValue + 1) / bucketTotalCount;
    for (NSNumber *element in unsorted) {
        NSInteger index = floor(([element doubleValue] - minValue) / space);
        [self insertElementTo:buckets[index] element:element];
    }
    // Create sorted array.
    NSMutableArray *sorted = [NSMutableArray array];
    for (NSArray *bucket in buckets) {
        [sorted addObjectsFromArray:bucket];
    }
    return sorted;
}

/// Insert a element to an array.
- (void) insertElementTo:(NSMutableArray *) array element:(NSNumber *) element {
    [array addObject:element];
    for(NSInteger i = array.count-1; i >= 1; i--) {
        if([array[i] integerValue] < [array[i-1] integerValue]) {
            [array exchangeObjectAtIndex:i withObjectAtIndex:i-1];
        }
    }
}
```

### 算法验证

```swift
let list = [21, 5, 322, 10, 623, 7111, 42, 56]
// 将会打印 [21, 5, 322, 10, 623, 7111, 42, 56]
print(list)
let list1 = bucketSort(unsortedArray: list)
// 将会打印 [5, 10, 21, 42, 56, 322, 623, 7111]
print(list1)
```