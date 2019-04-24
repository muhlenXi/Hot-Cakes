版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

## 排序
排序就是将一组对象按照某种逻辑顺序重新排列的过程。

# 选择排序

[选择排序](https://zh.wikipedia.org/wiki/%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F)（Selection Sort） 是一种简单直观的排序算法。

排序算法（默认是升序）的原理是这样的。首先，找到数组中最小的那个元素，将它与数组中的第一个元素交换位置。（如果第一个元素就是最小元素，则自己和自己交换。）其次，在剩下的元素中找到最小的元素，将它与数组中的第二个元素交换位置。如此循环，直到将整个数组排序。

对于长度为 $n$  的数组，选择排序需要大约  $n^2/2$ 次比较和 $n$ 次交换。

## 算法实现

用 Swift 实现的选择排序代码如下所示：

```swift
/// 选择排序
func selectionSort(unsortedArray: inout [Int]) {
    guard unsortedArray.count > 1 else {
        return
    }
    
    for i in 0 ..< unsortedArray.count {
        var minIndex = i
        for j in i+1 ..< unsortedArray.count {
            if unsortedArray[j] < unsortedArray[minIndex] {
                minIndex = j
            }
        }
        if minIndex != i {
            unsortedArray.swapAt(i, minIndex)
        }
    }
}
```

用 Objective-C 语言实现的算法如下：

```objc
- (NSArray*) selectionSort: (NSArray *) unsortedArray {
    if (unsortedArray.count <= 1) {
        return unsortedArray;
    }
    
    NSMutableArray *sortedArray = [unsortedArray mutableCopy];
    NSInteger minValueIndex = 0;
    
    for (NSInteger i = 0; i < sortedArray.count-1; i++) {
        minValueIndex = i;
        for (NSInteger j = i + 1; j < sortedArray.count; j++) {
            if ([sortedArray[j] integerValue] < [sortedArray[minValueIndex] integerValue]) {
                minValueIndex = j;
            }
        }
        if (minValueIndex != i) {
            [sortedArray exchangeObjectAtIndex:minValueIndex withObjectAtIndex:i];
        }
    }
    return [sortedArray copy];
}
```

## 验证算法

```swift
var list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
// 将会打印 [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
print(list) 
// 进行选择排序
selectionSort(unsortedArray: &list)
// 将会打印 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list) 
```

## 算法分析

`稳定性` ：是稳定算法，因为排序过程中相邻会依次比较，不会打乱相同元素的相对位置。

`空间复杂度`：整个排序过程是在原数组上进行排序的，所以是 O($1$)。

`时间复杂度`：排序算法包含双层嵌套循环，故为 O（$n^2$）。