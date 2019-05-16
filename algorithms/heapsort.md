### 铺垫知识

想要了解堆排序，我们先要学习几个概念。

**什么是二叉树?** 二叉树是每个节点最多有两个子树的树结构。节点左边的子树称为左子树，节点右边的子树称为右子树。

**什么是满二叉树？** 满二叉树指的是除了最后一层（叶节点）外，每个节点都有两个子节点的二叉树。一个 k 层的满二叉树一共有 2^k-1 个节点。

**什么是完全二叉树？** 完全二叉树的每层都完全填满，在最后一层上如果不是满的，则只缺少右边的若干节点。

**什么是堆？**  堆是一种特殊的完全二叉树，堆中的父节点永远大于或小于它的子节点。我们分别称之为最大堆或最小堆。也就是说最大堆中，每个父节点的值都大于它的子节点。在最小堆中，每个父节点的值都小于它的子节点的值。


### 堆排序

堆排序的原理主要是利用了堆的特性。在用数组表示的堆中，第一个元素就是该数组中值最大的元素。堆排序，主要分为以下三个步骤：

- 1、将要排序的数组构建成堆。
- 2、从数组的最后一个元素 n 开始，依次将第 0 个元素与该元素交换，然后再继续构建最大堆。
- 3、重复步骤 2，直到第 0 个元素。


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

用 Objective-C 实现的算法如下：

```objc
- (void)heapSort:(NSMutableArray *) tree {
    [self buidMaxHeap:tree];
    NSInteger last = tree.count-1;
    while (last >= 0) {
        [self swap:tree i:0 j:last];
        [self heapfy:tree n:last i:0];
        last--;
    }
}

- (void)buidMaxHeap:(NSMutableArray *) tree {
    NSInteger parent = (tree.count-1-1)/2;
    while (parent >= 0) {
        [self heapfy:tree n:tree.count i:parent];
        parent--;
    }
}

- (void)heapfy:(NSMutableArray *)tree n:(NSInteger)n i:(NSInteger)i {
    if (i >= n) {
        return;
    }
    
    NSInteger max = i;
    NSInteger c1 = i * 2 + 1;
    NSInteger c2 = i * 2 + 2;
    
    if (c1 < n && [tree[c1] integerValue] > [tree[max] integerValue]) {
        max = c1;
    }
    if (c2 < n && [tree[c2] integerValue] > [tree[max] integerValue]) {
        max = c2;
    }
    if (max != i) {
        [self swap:tree i:max j:i];
        [self heapfy:tree n:n i:max];
    }
}

- (void)swap:(NSMutableArray *)array i:(NSInteger)i j:(NSInteger)j {
    NSNumber *temp = array[i];
    array[i] = array[j];
    array[j] = temp;
}
```

### 算法验证

```swift
var list = [2, 3, 5, 7, 4, 8, 6 ,10 ,1, 9]
heapSort(tree: &list)
// Will print [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list)
```
