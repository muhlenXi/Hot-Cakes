版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

## 快速排序

[快速排序](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)（Quick Sort）简称快排，是由 C. A. R. Hoare 提出的一种排序算法。

快速排序的原理是使用 分治（Divide and conquer ）策略来把要排序的数组分为两个子序列，然后递归的分组并排序两个子序列，直到数组完成排序。

本文为了逻辑清晰，选择序列中间元素为基准值（pivot），当然这个基准值是可以任意选取的。

一个长度为 $n$ 的序列，平均时间复杂度为 $O(n log n)$，最坏的情况下复杂度为 $O(n^2)$。

## 算法实现

用 Swift 实现的算法如下：

```swift
/// 快速排序
func quickSort(unsortedArray: inout [Int], leftIndex: Int, rightIndex: Int){
    guard unsortedArray.count > 1 else {
        return
    }
    
    let pivot = unsortedArray[(leftIndex+rightIndex)/2]
    var i = leftIndex
    var j = rightIndex
    while i < j {
        while unsortedArray[i] < pivot {
            i += 1
        }
        while unsortedArray[j] > pivot {
            j -= 1
        }
        if i <= j {
            if i != j {
                unsortedArray.swapAt(i, j)
            }
            i += 1
            j -= 1
        }
    }
    
    if leftIndex < j {
        quickSort(unsortedArray: &unsortedArray, leftIndex: leftIndex, rightIndex: j)
    }
    if i < rightIndex {
        quickSort(unsortedArray: &unsortedArray, leftIndex: i, rightIndex: rightIndex)
    }
}
```

用 Objective-C 实现的算法如下：

```objc
- (void)quickSort:(NSMutableArray*) unsorted leftIndex:(NSInteger) leftIndex rightIndex:(NSInteger) rightIndex {

    NSInteger i = leftIndex;
    NSInteger j = rightIndex;
    NSInteger pivot = [unsorted[(leftIndex+rightIndex)/2] integerValue];
    while (i <= j) {
        while ([unsorted[i] integerValue] < pivot) {
            i++;
        }
        while ([unsorted[j] integerValue] > pivot) {
            j--;
        }
        if (i <= j) {
            [unsorted exchangeObjectAtIndex:i withObjectAtIndex:j];
            i++;
            j--;
        }
    }
    // 递归快排右边分组序列
    if (i < rightIndex) {
        [self quickSort:unsorted leftIndex:i rightIndex:rightIndex];
    }
    // 递归快排左边分组序列
    if (leftIndex < j) {
        [self quickSort:unsorted leftIndex:leftIndex rightIndex:j];
    }
}
```

## 验证算法

```swift
// 将会打印 [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
var list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
// 进行快速排序
quickSort(unsortedArray: &list, leftIndex: 0, rightIndex: list.count-1)
// 将会打印 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list)
```

## 算法分析

`稳定性`：非稳定性算法

`空间复杂度`：实现不同，空间复杂度不同。

`时间复杂度`：选取的基准值不同，时间复杂度也不同。选取最大值或最小值为基准值，时间复杂度为 $O(n^2)$。选取中间值为基准值，时间复杂度则为 $O(nlogn)$。