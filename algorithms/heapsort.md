### 算法实现

用 Swift 实现的堆排序如下：

```swift
func heapfy(tree: inout [Int], n: Int, i: Int) {
    if i >= n {
        return
    }
    
    var max = i
    let c1 = i * 2 + 1
    let c2 = i * 2 + 2
    if c1 < n && tree[c1] > tree[max] {
        max = c1
    }
    if c2 < n && tree[c2] > tree[max] {
        max = c2
    }
    if max != i {
        tree.swapAt(max, i)
        heapfy(tree: &tree, n: n, i: max)
    }
}

func buildMaxHeap(tree: inout [Int]) {
    var parent = (tree.count-1-1)/2
    while parent >= 0 {
        heapfy(tree: &tree, n: tree.count, i: parent)
        parent -= 1
    }
}


func heapSort(tree: inout [Int]) {
    buildMaxHeap(tree: &tree)
    var last = tree.count-1
    while last >= 0 {
        tree.swapAt(0, last)
        heapfy(tree: &tree, n: last, i: 0)
        last -= 1
    }
}
```

### 算法验证

```swift
var list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
heapSort(tree: &list)
// Will print [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list)
```
