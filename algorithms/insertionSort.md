版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

# 插入排序

[插入排序](https://zh.wikipedia.org/wiki/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)（Insertion Sort） 是一种简单直观的排序算法。

插入排序的灵感可能来自于我们玩牌的经验。当我们摸牌的时候，每摸到一张牌，都会将这张牌插入到其他有序的牌中的适当位置。

排序算法（默认是升序）的原理是，计算机在遍历数组中的元素时，认为当前索引左边的元素都是有序的，首先在左边有序的数组中从后往前找出当前数组应该插入的位置，然后依次挪动左边有序元素，给要插入的元素腾出位置，最后将该元素插入到腾空的位置。循环往复，直到数组完成排序。

对于长度为 $n$  的数组，选择排序需要大约  $n^2/2$ 次比较和 $n^2/2$ 次交换。

## 算法实现

用 Swift 实现的插入排序代码如下所示：

##### 实现一 ： 找出插入位置，然后移动元素，最后插入元素

```swift
/// 插入排序  先挪位置，然后插入
func insertionSort(unsortedArray: inout [Int]){
    guard unsortedArray.count > 1 else{
        return
    }
    
    for i in 1 ..< unsortedArray.count {
        let insertValue = unsortedArray[i]
        var formerIndex = i - 1
        while formerIndex >= 0 && unsortedArray[formerIndex] > insertValue{
            unsortedArray[formerIndex+1] = unsortedArray[formerIndex]
            formerIndex -= 1
        }
        if formerIndex != i - 1 {
             unsortedArray[formerIndex+1] = insertValue
        }
    }
}
```

##### 实现二：使用元素交换来插入元素

```swift
///  插入排序 使用交换来代替挪位置
func insertionSort1(unsortedArray: inout [Int]) {
    guard unsortedArray.count > 1 else{
        return
    }

    for i in 1 ..< unsortedArray.count {
        var formerIndex = i - 1
        while formerIndex >= 0 {
            if unsortedArray[formerIndex] > unsortedArray[formerIndex+1] {
                unsortedArray.swapAt(formerIndex, formerIndex+1)
            }
            formerIndex -= 1
        }
    }
}
```

用 Objective-C 实现的算法如下：

```objc

/// 先移位 再插入
- (NSArray*) insertionSort: (NSArray *) unsortedArray {
    if (unsortedArray.count <= 1) {
        return unsortedArray;
    }
    
    NSMutableArray *sortedArray = [unsortedArray mutableCopy];
    for (NSInteger i = 1; i < sortedArray.count; i++) {
        NSNumber *insertValue = sortedArray[i];
        NSInteger insertIndex = i-1;
        while (insertIndex >= 0 && [sortedArray[insertIndex] integerValue] > [insertValue integerValue]) {
            sortedArray[insertIndex+1] = sortedArray[insertIndex];
            insertIndex--;
        }
        
        if (insertIndex != i - 1) {
            sortedArray[insertIndex+1] = insertValue;
        }
    }
    
    return [sortedArray copy];
}

/// 利用交换代替插入
- (NSArray *) insertionSort1: (NSArray *) unsortedArray {
    if (unsortedArray.count < 2) {
        return unsortedArray;
    }
    
    NSMutableArray *sortedArray = [unsortedArray mutableCopy];
    for (NSInteger i = 1; i < sortedArray.count; i++) {
        NSInteger j = i - 1;
        while (j >= 0 && [sortedArray[j] integerValue] > [sortedArray[j+1] integerValue]) {
            [sortedArray exchangeObjectAtIndex:j withObjectAtIndex:j+1];
            j--;
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
// 用算法一进行插入排序
insertionSort(unsortedArray: &list)
// 将会打印 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list)
// 恢复数组排序，用算法二排序
list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
insertionSort1(unsortedArray: &list)
// 将会打印 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list) 
```

## 算法分析

`稳定性` ：是稳定算法，因为排序过程中相邻会依次比较，不会打乱相同元素的相对位置。

`空间复杂度`：整个排序过程是在原数组上进行排序的，所以是 O($1$)。

`时间复杂度`：排序算法包含双层嵌套循环，故为 O（$n^2$）。

## 应用
插入排序适合于部分有序的数组。