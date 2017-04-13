### 数组知识点

### 不可变数组 NSArray

#### 1、通过对象来创建数组

```objc
NSArray *arr1 = [[NSArray alloc] initWithObjects:@"string1",@"sting2",@"string3",nil];
// 类方法
NSArray *arr3 = [NSArray arrayWithObjects:@"1",@"2",@"3", nil]; 
// 一个空的数组
NSArray *arr5 = [NSArray array];  
```

#### 2、通过一个数组来创建

```objc
NSArray *arr2 = [[NSArray alloc] initWithArray:arr1];
NSArray *arr4 = [NSArray arrayWithArray:arr3];
```
    
#### 3、只适合不可变数组  数组里面的元素都是对象  数组是固定的 元素的位置固定的

```objc
Cat *cat = [[Cat alloc] init];
NSArray *arr6 = @[arr2,@"字符串",cat];
```

*创建一个空对象*

NSNull *nulObject = [[NSNull alloc] init];

#### 4、获取数组的长度

```objc
NSUInteger count = [arr count];
NSLog(@"count == %lu",count);
```
    
#### 5、通过下标获取对应的元素
    
```objc
NSString *str = [arr objectAtIndex:1];	
//方式1    
NSString *str1 = arr[2];     //方式2  
```
  
#### 6、获取数组最后一个元素

```objc
NSString *str3 = [arr lastObject];
```
    
#### 7、通过数组的元素来获取对应的下标

```objc
NSUInteger index = [arr indexOfObject:@"3"];
```
   
#### 8、判断数组中是否包含某个元素

```objc
BOOL ret = [arr containsObject:@"4"];
```
        
#### 9、根据范围取子数组 subarrayWithRange

```objc
NSRange range = NSMakeRange(0, 2);
NSArray *arr1 = [arr subarrayWithRange:range];
```
    
#### 10、任意取数组下标组成一个子数组 objectsAtIndexes

```objc
// NSMutableIndexSet 数组下标集合
NSMutableIndexSet *indexs = [[NSMutableIndexSet alloc] init];
[indexs addIndex:0];
[indexs addIndex:3];
[indexs addIndex:4];
NSArray *subArr2 = [arr objectsAtIndexes:indexs];
```
    
#### 11、判别对象是属于哪个类 isKindOfClass 
    
```objc
if ([str isKindOfClass:[NSString class]]) 
	NSLog(@"str 是字符串类 ");
```

### 可变数组  NSMutableArray

#### 1、对象方法初始化

```objc
NSMutableArray *arrM = [[NSMutableArray alloc] initWithObjects:@"1",@"2",@"3",@"4",@"5", nil];
    	NSArray *arr = @[@"one",@"two"];	
    	
// 通过数组初始化
NSMutableArray *arrM2 = [[NSMutableArray alloc] initWithArray:arr]
// 初始化一个空的数组
NSMutableArray *arrM5 = [[NSMutableArray alloc] init];  

// 类方法初始化
NSMutableArray *arrM1 = [NSMutableArray arrayWithObjects:@"1",@"2",@"3", nil]; 
// 通过数组初始化  
NSMutableArray *arrM3 = [NSMutableArray arrayWithArray:arr];

//是一个参考容量，数组会根据实际的元素变换长度 
NSMutableArray *arrM4 = [NSMutableArray arrayWithCapacity:0]; 
 
// 实例化一个空的可变数组   <重点>    	
NSMutableArray *arrM = [NSMutableArray array];
```  
#### 2、添加一个对象 <重点>

```objc
[arrM addObject:@"xiaoming"];
```
      
#### 3、添加一个数组对象

```objc
[arrM addObjectsFromArray:@[@"xiaowang",@"laowang"]];
```

#### 4、插入一个元素 <重点>

```objc
[arrM insertObject:@"wangmazi" atIndex:1];
```
       
#### 5、插入多个元素   
 
*对象个数 和 整数个数要一致相等*

```objc
NSMutableIndexSet *index = [[NSMutableIndexSet alloc] init];
[index addIndex:2];
[index addIndex:4];
[arrM insertObjects:@[@"ernai",@"xiaosan"]  atIndexes:index];
```
  
#### 6、删除指定下标的元素 <重点>

```objc
[arrM removeObjectAtIndex:1];
//删除数组的最后一个元素
[arrM removeLastObject];  
```
    
#### 7、指定对象来删除元素

```objc
[arrM removeObject:@"ernai"];
```
    
#### 8、在一定范围删除指定的元素

```objc
[arrM removeObject:@"xiaosan" inRange:NSMakeRange(0, 3)];
```
   
#### 9、删除指定范围内的所有元素

```objc
[arrM removeObjectsInRange:NSMakeRange(0, 2)];
```
    
#### 10、根据一个数组删除指定的元素

```objc
[arrM removeObjectsInArray:@[@"1",@"2"]];
```
   
#### 11、一次性删除所有的下标集合元素《重点》

```objc
NSMutableIndexSet *index1 = [[NSMutableIndexSet alloc] init];
[index1 addIndex:1];
[index1 addIndex:5];
[arrM removeObjectsAtIndexes:index1];
```
    
#### 12、删除所有元素

```objc
[arrM removeAllObjects];
```
    
#### 13、交换数组内的两个元素

```objc
[arrM addObjectsFromArray:@[@"1",@"2",@"3",@"4",@"5",@"6",@"7"]];
[arrM exchangeObjectAtIndex:2 withObjectAtIndex:6];
```
   
#### 14、替换一个元素

```objc
[arrM replaceObjectAtIndex:1 withObject:@"10"];
```
    
#### 15、替换多个元素
   
```objc
NSMutableIndexSet *index2 = [[NSMutableIndexSet alloc] init];
[index2 addIndex:2];
[index2 addIndex:5];
[arrM replaceObjectsAtIndexes:index2  withObjects:@[@"20",@"30"]];
```
       
#### 16、全部修改 重置数组

```objc
[arrM setArray:@[@"fine"]];
```

#### 17、数组对象排序

*sortedArrayUsingSelector 按照ASC码大小进行排序的*

*@selector选择器   选择器里面选择方法*

```objc
NSMutableArray *arrM = [NSMutableArray arrayWithObjects:@"A",@"F",@"X",@"B",@"C", nil];

NSArray *arr = [arrM sortedArrayUsingSelector:@selector(compare:)];
```
    
#### 18、数组的遍历

*方法一：for循环*

*方法二：OC枚举器*

1）正序输出所有元素

```objc
NSEnumerator *enumerator = [arr objectEnumerator];
id obj = nil;
while (1) {
   	obj = [enumerator nextObject];  //就是从枚举器里面一个一个的取出元素
    NSLog(@"obj == %@",obj);
    if (obj == nil) {
    	break;
    }
}
```

2）逆序输出所有元素

```objc
enumerator = [arr reverseObjectEnumerator];
while (1) {
obj = [enumerator nextObject];
NSLog(@"obj == %@",obj);
if (obj == nil) {
    	break;
	}
}
```

*方法三：快速枚举法*

```objc
for (id goods in arr) {
	NSLog(@"goods == %@",goods);
}
```

#### 19、将字符串按照指定的字符串进行分割

```objc
NSArray *arr = [str componentsSeparatedByString:@"+"];
```
    
*按切割字符的集合切割*

```objc
NSCharacterSet *set = [NSCharacterSet characterSetWithCharactersInString:@"*+/-"];
NSArray *arr1 = [str1 componentsSeparatedByCharactersInSet:set];
```
    
#### 20、把数组里面的元素拼接成字符串

```objc
NSArray *arr2 = @[@"2015",@"12",@"04",@"星期五"];
NSString *str2 = [arr2 componentsJoinedByString:@"-"];
``` 
