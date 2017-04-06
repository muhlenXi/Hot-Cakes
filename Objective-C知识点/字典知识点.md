###  字典知识点
	
### 不可变字典 NSDictionary

#### 1、创建一个字典对象  实例化方法

*对象方法创建*

```objc
NSDictionary *dic = [[NSDictionary alloc] initWithObjectsAndKeys:@"1",@"one",@"2",@"two",@"3",@"three", nil]; 
   
// 键是数组,值也是数组
NSDictionary *dic1 = [[NSDictionary alloc] initWithObjects:@[@"1",@"2",@"3"]forKeys:@[@"one",@"two",@"three"]];

// 根据一个字典创建
NSDictionary *dic2 = [[NSDictionary alloc] initWithDictionary:dic];
```
*类方法：*

```objc
NSDictionary *dic3 = [NSDictionary dictionaryWithObjectsAndKeys:@"1",@"one",@"2",@"two",@"3",@"three", nil];

NSDictionary *dic4 = [NSDictionary dictionaryWithObjects:@[@"1",@"2",@"3"] forKeys:@[@"one",@"two",@"three"]];
NSDictionary *dic5 = [NSDictionary dictionaryWithDictionary:dic];
```

*不可变字典独有的创建方式*

```objc
NSDictionary *dic6 = @{@"one":@"1",@"two":@"2",@"three":@"3"};
```
  
#### 2、字典长度

```objc
NSUInteger count = [dic count];
```
    
#### 3、通过key取value

```objc
NSString *str = [dic valueForKey:@"小白"];
NSString *str1 = dic[@"小黑"];
```

#### 3、获取字典中所有key

```objc
NSArray *keys = [dic allKeys];
```

#### 4、获取字典中所有value

```objc
NSArray *values = [dic allValues];
```
    
#### 5、通过一个value获取所对应的所有key

```objc
NSArray *arr2 = [dic allKeysForObject:@"刘备"];
```

### 可变字典 NSMutableDictionary
   
#### 1、创建一个空的可变字典
 
```objc
NSMutableDictionary *dicM = [[NSMutableDictionary alloc] initWithCapacity:0];
```
	
#### 2、增加一对键值对<重点>

```objc
NSDictionary *dic = @{@"two":@"2",@"three":@"3",@"four":@"4",@"five":@"5"};
[dicM setObject:@"1" forKey:@"one"];
 
// 增加整个字典
[dicM addEntriesFromDictionary:dic];
```

#### 3、通过key删除这对键值对<重点>

```objc
[dicM removeObjectForKey:@"two"];
```
  
#### 4、通过制定的一个数组key来删除所对的键值对

```objc
[dicM removeObjectsForKeys:@[@"three",@"one"]];
```
     
#### 5、删除字典中所有的键值对

```objc
[dicM removeAllObjects];
```

#### 6、修改整个字典

```objc
[dicM setDictionary:@{@"one":@"1",@"two":@"2"}];
```
    
#### 7、修改键值对

```objc
[dicM setObject:@"5" forKey:@"one"];
```
   


