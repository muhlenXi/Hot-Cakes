## 算法实现

用 Swift 实现的算法如下：

```swift
    func countingSort(unsortedArray: [Int]) -> [Int]{
        guard unsortedArray.count > 2 else {
            return unsortedArray
        }
        // 1、Find max and min value.
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
        // 2、Create an array which element is zero.
        let totalCount = maxValue - minValue + 1
        var countingArray = [Int]()
        for _ in 0..<totalCount {
            countingArray.append(0)
        }
        // 3、Count the number of occurrences of elements in an unsorted array.
        for element in unsortedArray {
            let index = element - minValue
            countingArray[index] += 1
        }
        // 4、Create an sorted array from counting array.
        var sortedArray = [Int]()
        for (index, element) in countingArray.enumerated() {
            var count = 0
            while count < element {
                sortedArray.append(index+minValue)
                count += 1
            }
        }
        return sortedArray;
    }
```

用 Objective-C 实现的算法如下：

```objc
- (NSArray*)countingSort:(NSArray*) unsorted {
    if (unsorted.count < 2) {
        return unsorted;
    }
    // 1、获取待排序数组中的最大值和最小值
    NSInteger maxValue = [unsorted[0] integerValue];
    NSInteger minValue = [unsorted[0] integerValue];
    for (NSNumber *number in unsorted) {
        if ([number integerValue] > maxValue) {
            maxValue = [number integerValue];
        }
        if ([number integerValue] < minValue) {
            minValue = [number integerValue];
        }
    }
    
    // 2、初始化一个存放待排序数组出现元素次数的数组，初始值为0
    NSInteger totalCount = maxValue-minValue+1;
    NSMutableArray *countingArray = [NSMutableArray array];
    NSInteger index = 0;
    while (index < totalCount) {
        [countingArray addObject:@(0)];
        index++;
    }
    // 3、统计待排序数组中元素出现的次数
    for (NSNumber *number in unsorted) {
        NSInteger index = [number integerValue] - minValue;
        NSInteger count = [countingArray[index] integerValue];
        countingArray[index] = @(count+1) ;
    }
    // 4、根据数组元素出现次数数组生成排序后的数组
    NSMutableArray *sortedArray = [NSMutableArray array];
    for (NSInteger index = 0; index < countingArray.count; index++) {
        NSInteger count = 0;
        while (count < [countingArray[index] integerValue]) {
            [sortedArray addObject:@(minValue+index)];
            count++;
        }
    }
    
    return [sortedArray copy];
}
```

## 算法验证

```swift
let list = [8, 1, 4, 6, 2, 3, 5, 7]
// will print [8, 1, 4, 6, 2, 3, 5, 7]
print(list)
let list1 = countingSort(unsortedArray: list)
// will print [1, 2, 3, 4, 5, 6, 7, 8]
print(list1)
```