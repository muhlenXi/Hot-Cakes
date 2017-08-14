### 沙盒 知识点

#### 1、获取沙盒的路径 HomeDirectory

```objc
NSString *sandBoxPath = NSHomeDirectory();
```

#### 2、获取Documents的路径

*一般保存重要的数据，会进行云备份的*

```objc
NSString *documentsPath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask, YES)[0];
```

#### 3、获取Library的路径

```objc
NSString *libraryPath = NSSearchPathForDirectoriesInDomains(NSLibraryDirectory,NSUserDomainMask, YES)[0];
```
	
#### 4、获取caches的路径

*放缓存的数据（可以删除的）*

```objc
NSString *cachesPath = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
```

#### 5、获取tmp的路径

*临时文件夹，每一次关闭程序都会清空*

```objc
NSString *tmpPath = NSTemporaryDirectory();
```

#### 6、Dictionary 读写操作

```objc
NSDictionary *dic = [NSDictionary dictionary];
//写
[dic writeToFile:@"documents/xxx.plist" atomically:YES];  
//读    
[NSDictionary dictionaryWithContentsOfFile:@"documents/xxx.plist"];  
```

#### 7、读取数据

*假设数据保存在Documents中(documents/userInfo.plist)*

```objc
// 1、得到Documents的路径
NSString *path = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES)[0];

// 2、拼接路径
 path = [path stringByAppendingPathComponent:@"userInfo.plist"];

// 3.从该路径读取数据
NSMutableArray *userArray = [NSMutableArray arrayWithContentsOfFile:path];
    
[NSMutableDictionary dictionaryWithContentsOfFile:path]  // 读字典
```


#### 8、将数组写入沙盒

```objc
[userArray writeToFile:path atomically:YES];
```



