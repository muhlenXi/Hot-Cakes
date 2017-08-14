### 解档与归档 知识点

#### 1、归档方法：

```objc
BOOL ret = [NSKeyedArchiver archiveRootObject:self.arrM toFile:path1];
if (ret) {
	 NSLog(@"归档成功");
} else {
	NSLog(@"归档失败");
}
```

#### 2、解档方法:

```objc
NSArray * arr = [NSKeyedUnarchiver unarchiveObjectWithFile:path1];
if(arr != nil) {
	NSLog(@"解档成功");
}
```

#### 2、如果对模型归解档

**创建的模型需要遵循`NSCoding>`协议并且要实现相应方法**

```objc
// .h文件中声明
@interface Book : NSObject <NSCoding>   

// .m文件中实现

// 归档方法  
- (void)encodeWithCoder:(NSCoder *)aCoder
{
    	[aCoder encodeObject:self.title forKey:@"title"];
    	[aCoder encodeObject:self.author forKey:@"author"];
    
}

// 解档方法
- (instancetype)initWithCoder:(NSCoder *)aDecoder
{
	self = [super init];
    	if (self) {
        	self.title = [aDecoder decodeObjectForKey:@"title"];
        	self.author = [aDecoder decodeObjectForKey:@"author"];
    	}
	return self;
}
```


