### Objective-C 代码技巧

#### 1、String 转为 NSMutableString


```objc
NSMutableString * mStr = [uuid mutableCopy];
[mStr deleteCharactersInRange:NSMakeRange(0, 1)];
NSLog(@"mStr == %@",mStr);
// print 020
```

