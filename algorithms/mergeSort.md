版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

## 归并排序

[归并排序](https://zh.wikipedia.org/wiki/%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F) （Merge Sort）是由 冯·诺伊曼 在 1945 年提出的算法。该算法也是采用 分治法 （Divide and Conquer）的策略。

归并排序的原理是，首先递归的把当前序列平均分成两组，然后在保持元素顺序的同时将分割后的子序列合并到一起。

一个长度为 $n$ 的序列，最坏和最好的情况下复杂度均为 $O(n log n)$。

## 算法实现

用 Swift 实现的算法如下：

```swift
// 归并
func mergeSort(unsortedArray: [Int]) -> [Int]{
    let len = unsortedArray.count
    if len <= 1 {
        return unsortedArray
    }
    let mid = len / 2
    let leftArray = mergeSort(unsortedArray: Array(unsortedArray[0..<mid]))
    let rightArray = mergeSort(unsortedArray: Array(unsortedArray[mid..<len]))
    
    var result = [Int]()
    var leftIndex = 0
    var rightIndex = 0
    while leftIndex < leftArray.count && rightIndex < rightArray.count {
        if leftArray[leftIndex] < rightArray[rightIndex] {
            result.append(leftArray[leftIndex])
            leftIndex += 1
        } else {
            result.append(rightArray[rightIndex])
            rightIndex += 1
        }
    }
    
    while leftIndex < leftArray.count {
        result.append(leftArray[leftIndex])
        leftIndex += 1
    }
    while rightIndex < rightArray.count {
        result.append(rightArray[rightIndex])
        rightIndex += 1
    }
    
    return result
}
```

用 Objective-C 实现的算法如下：

```objc
/// 归并排序
- (NSArray*) mergeSort: (NSArray *) unsortedArray {
    if (unsortedArray.count <= 1) {
        return unsortedArray;
    }
    // 拆分
    NSInteger mid = unsortedArray.count / 2;
    NSArray *leftArray = [self mergeSort:[unsortedArray subarrayWithRange:NSMakeRange(0, mid)]];
    NSArray *rightArray = [self mergeSort:[unsortedArray subarrayWithRange:NSMakeRange(mid, unsortedArray.count-mid)]];
    // 合并
    NSInteger leftIndex = 0;
    NSInteger rightIndex = 0;
    NSMutableArray *sortedArrray = [NSMutableArray array];
    while (leftIndex < leftArray.count && rightIndex < rightArray.count) {
        if ([leftArray[leftIndex] integerValue] > [rightArray[rightIndex] integerValue]){
            [sortedArrray addObject:rightArray[rightIndex]];
            rightIndex++;
        } else {
            [sortedArrray addObject:leftArray[leftIndex]];
            leftIndex++;
        }
    }
    // 添加左边剩余元素
    if (leftIndex < leftArray.count) {
        [sortedArrray addObjectsFromArray:[leftArray subarrayWithRange:NSMakeRange(leftIndex, leftArray.count-leftIndex)]];
    }
    // 添加右边剩余元素
    if (rightIndex < rightArray.count) {
        [sortedArrray addObjectsFromArray:[rightArray subarrayWithRange:NSMakeRange(rightIndex, rightArray.count-rightIndex)]];
    }
    
    return sortedArrray;
}
```

## 算法验证

```swift
var list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
// 将会打印 [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
print(list)
// 将会打印归并排序后的序列 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(mergeSort(unsortedArray: list))
```

## 算法分析

`稳定性`：稳定算法，序列分割的时候可能将相同元素分到不同组中，但是合并的时候相同元素的相对位置不会改变。

`空间复杂度`：额外需要一个数组来保存排序结果，所以为 $O(n)$。

`时间复杂度`：最好最坏都为 $O(nlogn)$。

