用 Objective-C 实现的算法如下：

```objc
- (NSArray*)bucketSort:(NSArray*) unsorted {
    if (unsorted.count < 2) {
        return unsorted;
    }
    // Find max and min value.
    double minValue = [unsorted[0] doubleValue];
    double maxValue = [unsorted[0] integerValue];
    for(NSNumber *element in unsorted) {
        if([element integerValue] > maxValue) {
            maxValue = [element integerValue];
        }
        if([element integerValue] < minValue) {
            minValue = [element integerValue];
        }
    }
    // Create buckets array
    NSInteger bucketTotalCount = 3;
    NSMutableArray *buckets = [NSMutableArray array];
    for(NSInteger i = 0; i < bucketTotalCount; i++) {
        [buckets addObject:[NSMutableArray array]];
    }
    // Add unsorted element to buckets array
    double space = (maxValue - minValue + 1) / bucketTotalCount;
    for (NSNumber *element in unsorted) {
        NSInteger index = floor(([element doubleValue] - minValue) / space);
        [self insertElementTo:buckets[index] element:element];
    }
    // Create sorted array.
    NSMutableArray *sorted = [NSMutableArray array];
    for (NSArray *bucket in buckets) {
        [sorted addObjectsFromArray:bucket];
    }
    return sorted;
}

/// Insert a element to an array.
- (void) insertElementTo:(NSMutableArray *) array element:(NSNumber *) element {
    [array addObject:element];
    for(NSInteger i = array.count-1; i >= 1; i--) {
        if([array[i] integerValue] < [array[i-1] integerValue]) {
            [array exchangeObjectAtIndex:i withObjectAtIndex:i-1];
        }
    }
}
```