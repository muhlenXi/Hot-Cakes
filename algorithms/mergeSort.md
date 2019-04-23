版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

## 归并排序

[归并排序](https://zh.wikipedia.org/wiki/%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F) （Merge Sort）是由 冯·诺伊曼 在 1945 年提出的算法。该算法也是采用 分治法 （Divide and Conquer）的策略。

归并排序的原理是，首先递归的把当前序列平均分成两组，然后在保持元素顺序的同时将分割后的子序列合并到一起。

一个长度为 $n$ 的序列，最坏和最好的情况下复杂度均为 $O(n log n)$。

## 算法实现

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

