版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

# 希尔排序

[希尔排序](https://zh.wikipedia.org/wiki/%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F)（Shell Sort）也被称为递减增量排序，是插入排序的一种高效改进版本算法，该算法由 Donald Shell 设计并在1959 年公布的。

希尔排序的原理是将数组中的全部元素按照步长序列依次分为 $m$ 个部分，然后分别在每个部分中进行插入排序，这样一个元素会一次性朝最终位置前进一大步。最后当步长序列取值为 $1$ 时，就是普通的插入排序。

步长的选择是希尔排序的重要部分。算法一开始以特定的步长进行排序，然后步长递减，最终递减为 $1$, 当步长为 1 时，算法变为普通插入排序，这样就保证了数据一定会被排序。

一个长度为 $n$ 的序列，选择不同的步长序列，在最坏情况下的时间复杂度是不同的。比如当步长序列为 $2^k - 1$ 时，时间复杂度为 $O(n^{3/2})$。

## 算法实现

本次用 Swift 实现的希尔排序步长序列为 $n/2^i$，如下所示：

```swift
/// 希尔排序
func shellSort(unsortedArray: inout [Int]) {
    guard unsortedArray.count > 1 else {
        return
    }
    
    var gap = unsortedArray.count / 2
    while gap > 0 {
        for i in gap ..< unsortedArray.count {
            var formerIndex = i - gap
            while formerIndex >= 0 {
                if unsortedArray[formerIndex] > unsortedArray[formerIndex+gap] {
                    unsortedArray.swapAt(formerIndex, formerIndex+gap)
                }
                formerIndex -= gap
            }
        }
        gap = gap / 2
    }
}
```

用 Objective-C 实现的算法如下：

```objc
/// 希尔排序 插入排序改进版
- (NSArray *) shellSort: (NSArray *) unsortedArray {
    if (unsortedArray.count < 2) {
        return unsortedArray;
    }
    
    NSMutableArray *sortedArray = [unsortedArray mutableCopy];
    NSInteger gap = unsortedArray.count / 2;
    
    while (gap > 0) {
        for (NSInteger i = gap; i < sortedArray.count; i++) {
            NSInteger formal = i - gap;
            while (formal >= 0 && [sortedArray[formal] integerValue] > [sortedArray[formal+gap] integerValue]) {
                [sortedArray exchangeObjectAtIndex:formal withObjectAtIndex:formal+gap];
                formal -= gap;
            }
        }
        gap = gap / 2;
    }
    
    return sortedArray;
}
```


## 验证算法

```swift
var list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
// 将会打印 [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
print(list) 
// 进行希尔排序
shellSort(unsortedArray: &list)
// 将会打印 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list)
```

## 算法分析

`稳定性`：是非稳定性算法，因为可能会将相同元素分到不同的组中。
`空间复杂度`：$O(1)$，因为整个排序是在原数组上完成的。
`时间复杂度`：因步长序列的不同而不同。