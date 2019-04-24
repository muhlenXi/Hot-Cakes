版权声明：本文为 [muhlenXi](http://www.muhlenxi.com) 原创文章，未经博主允许不得转载，如有问题，欢迎指正。

# 冒泡排序
[冒泡排序](https://zh.wikipedia.org/wiki/%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F)（Bubble Sort）的排序方法是在每一轮排序过程中，依次比较相邻元素的大小，如果顺序不满足排序的要求，则交换这两个元素。这样每一轮排序结束后，都能将最大的元素的元素放到最后。

一个 包含 **n** 个元素的数组在最坏的情况下，需要进行 **n-1** 次排序过程，方能使数组中的元素变得有序。

## 算法实现

用 Swift 实现的冒泡排序代码如下所示：

```swift
/// 冒泡排序 升序
func bubbleSort(unsortedArray: inout [Int]){
    guard unsortedArray.count > 1 else{
        return 
    }
    
    for i in 0 ..< unsortedArray.count-1 {
        var exchanged = false
        for j in 0 ..< unsortedArray.count-1-i {
            if unsortedArray[j] > unsortedArray[j+1] {
                unsortedArray.swapAt(j, j+1)
                exchanged = true
            }
        }
        if !exchanged {
            break
        }
    }
}
```

用 Objective-C 实现的算法如下：

```objc
- (NSArray*) bubbleSort: (NSArray *) unsortedArray {
    if (unsortedArray.count <= 1) {
        return unsortedArray;
    }
    
    NSMutableArray *sortedArray = [unsortedArray mutableCopy];
    
    for (int i = 0; i < sortedArray.count-1; i++) {
        BOOL exchanged = NO;
        for (int j = 0; j< sortedArray.count-1-i; j++) {
            if ([sortedArray[j] integerValue] > [sortedArray[j+1] integerValue]) {
                [sortedArray exchangeObjectAtIndex:j withObjectAtIndex:j+1];
                exchanged = YES;
            }
        }
        if (!exchanged) {
            break;
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
// 进行冒泡排序
bubbleSort(unsortedArray: &list)
// 将会打印 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list) 
```

## 算法分析

`稳定性` ：是稳定算法，因为排序过程中相邻会依次比较，不会打乱相同元素的相对位置。

`空间复杂度`：整个排序过程是在原数组上进行排序的，所以是 O($1$)。

`时间复杂度`：排序算法包含双层嵌套循环，故为 O（$n^2$）。