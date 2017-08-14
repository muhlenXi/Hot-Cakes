### 字符串 知识点

### 不可变字符串 NSString

#### 1、不可变字符串创建方式

```objc
NSString *str10 = @"我是OC字符串";
```
    
#### 2、根据C字符串创建（c字符串转化为OC字符串）编码方式:NSUTF8StringEncoding

```objc
char *s = "abcd";
NSString *str = [NSString stringWithCString:s encoding:NSUTF8StringEncoding];
NSLog(@"str == %@",str);
```
    
#### 3、根据拼接来创建 <重点>

*快速的把i和f拼接为一个OC字符串*

```objc
NSInteger i = 10;
CGFloat f = 9.8;
NSString *str1 = [NSString stringWithFormat:@"%ld-拼接-%f",i,f];
```

#### 4、通过c字符串UTF8String函数创建OC字符串
	
```objc
NSString *str4 = [[NSString alloc] initWithUTF8String:s];
```
  
#### 5、通过现有的OC字符串创建一个OC字符串

```objc
NSString *str5 = [[NSString alloc] initWithString:str4];
```
        
#### 6、通过类方法通过现有的OC字符串创建一个OC字符串

```objc
NSString *str6 = [NSString stringWithString:str3];
```

#### 7、比较两个字符串内容是否相同 <重点>  返回的是一个BOOL值

```objc
[str isEqualToString:str1] 
```    
    
#### 8、length长度只算字符里面的个数 <重点>

```objc
NSString *str2 = @"我们都是好孩子wewe";
NSUInteger count = [str2 length];
```   

*c语言获取字符串长度   strlen(s) c语言一个汉字占三个长度*
     
#### 9、字符串转化为数字

```objc
NSString *str3 = @"32132837287";
double value = [str3 doubleValue];
```
    
#### 10、字符串转化为BOOl 类型

```objc
NSString *str4 = @"1";
BOOL ret = [str4 boolValue];
```

#### 11、大小写转换

```objc
NSString *str5 = @"sdadhSDADSDKJhkad";
//全部转换为大写字母
NSLog(@"str5大写 == %@",[str5 uppercaseString]);//全部转换为小写字母			
NSLog(@"str5小写 == %@",[str5 lowercaseString]);
```			
    
#### 12、判断前后缀

```objc
NSString *str6 = @"http://www.baidu.com";
if ([str6 hasPrefix:@"http:"]) 
	NSLog(@"存在http:的前缀");
if ([str6 hasSuffix:@".com"]) 
	NSLog(@"存在.com的后缀");
```
   
#### 13、字符串拼接 <重点>

```objc
NSString *str7 = [str6 stringByAppendingString:@"我爱中国"];   
NSString *str8 = [str6 stringByAppendingPathComponent:@".cn"];
```
    
#### 14、比较两个字符串的大小 <重点>
    
```objc
NSComparisonResult result =  [str compare:str1];
if (result == NSOrderedAscending) {
	NSLog(@"str 小于 str1");
} else if (result == NSOrderedSame) {
	NSLog(@"str 等于 str1");
} else {
	NSLog(@"str 大于 str1");
}
```
    
#### 15、取出指定位置的字符  提取字串

```objc
NSString *str11 = @"我们都是super man";
unichar c =  [str11 characterAtIndex:0];
NSLog(@"c == %C",c);
```
    
#### 16、从指定位置开始向后提取字串(一直提取到最后) <重点>

```objc
NSString *str12 = [str11 substringFromIndex:2];  //闭区间
NSLog(@"str12 == %@",str12);
```
    
#### 17、从开始到指定位置下标提取字串  <重点>

```objc
NSString *str13 = [str11 substringToIndex:9];   //开区间
NSLog(@"str13 == %@",str13);
```
    
#### 18、在字符串里面查找字串

*从左边到右边找*

```objc    
NSRange range = [str11 rangeOfString:@"man"];
if (range.location == NSNotFound) {
	NSLog(@"该字符串找不到这个子串");
} else {
	NSLog(@"range.location == %lu range.length == %lu",range.location,range.length);
}
```    
*从右边到左边找加一个参数 `NSBackwardsSearch`。*

```objc 
NSRange range1 = [str11 rangeOfString:@"super" options:NSBackwardsSearch];
if (range1.location != NSNotFound) {
	NSLog(@"range.location == %lu range.length == %lu",range1.location,range1.length);
}
```
    
#### 19、substringWithRange提取区域内的字串

```objc
NSString *str14 =[str11 substringWithRange:range];
NSLog(@"str14 == %@",str14);
```
    
### 可变字符串 NSMutableString 继承于 NSString
    
#### 1、创建一个NSMutableString

```objc
NSMutableString *str = [[NSMutableString alloc] initWithFormat:@"I am good boy"];
NSMutableString *str1 = [[NSMutableString alloc] initWithString:@"hello"];
```

#### 2、重置可变字符串内容

```objc
NSMutableString *str1 = [NSMutableString string];
[str setString:@"you are beautiful girl"];
``` 
    
#### 3、在可变字符串后面添加内容

```objc
[str appendString:@" and clever"];
```
        
#### 4、格式化追加内容<重点>

```objc
NSInteger age = 10;
[str appendFormat:@" %ld",age];
```
    
#### 5、将指定字符串插入到目标字符串指定位置

```objc
[str1 insertString:@"喜欢卡特琳娜" atIndex:9]
```
   
#### 6、删除NSRange指定范围中的字符串

```objc
NSRange range = NSMakeRange(0, 5);
[str1 deleteCharactersInRange:range];
```
    
#### 7、在指定范围中 替换 字符串

```objc
NSRange range2 = [str1 rangeOfString:@"26M"];
[str1 replaceCharactersInRange:range2 withString:@"喜欢小萝莉"];
```


