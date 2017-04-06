### 文件管理 
       
#### 1、实例化一个文件管理对象

```objc
NSFileManager *fileM = [NSFileManager defaultManager];
```

#### 2、contentsOfDirectoryAtPath获取指定目录中的所有内容
    
```objc
NSArray *arr = [fileM contentsOfDirectoryAtPath:@"/Users/liuguilin/Desktop/OC1535班" error:&error];
```

#### 3、遍历出指定目录中所有文件,包括子目录 subpathsOfDirectoryAtPath

```objc
NSArray *arr1 = [fileM subpathsOfDirectoryAtPath:@"/Users/liuguilin/Desktop/OC1535班" error:nil];
```

#### 4、查找后缀名

```objc
[[obj pathExtension] isEqualToString:@"rtf"];
```

#### 5、判断文件是否存在

```objc
[fileM fileExistsAtPath:PATH(@"1")];
```

#### 6、指定路径创建文件夹

```objc
BOOL ret = [fileM createDirectoryAtPath:PATH(@"1/2/3") withIntermediateDirectories:YES attributes:nil  error:nil];
```

#### 7、指定路径下创建文件

```objc
BOOL ret = [fileM createFileAtPath:@"/Users/liuguilin/Desktop/1/123.mp3" contents:nil attributes:nil];
```

#### 8、拷贝文件夹或文件  注意：目的地需要添加名字 不然操作都会失败

```objc
BOOL ret = [fileM copyItemAtPath:@"/Users/liuguilin/Desktop/联系人文件.txt" toPath:@"/Users/liuguilin/Desktop/1/联系人" error:nil];
```

#### 9、移动文件

```objc
BOOL ret = [fileM moveItemAtPath:@"/Users/liuguilin/Desktop/21" toPath:@"/Users/liuguilin/Desktop/1/" error:nil];
```

#### 10、删除文件

```objc
BOOL ret = [fileM removeItemAtPath:@"/Users/liuguilin/Desktop/联系人文件" error:nil];
```

#### 11、沙盒document路径获取

```objc
NSString *homePath = NSHomeDirectory();
NSArray *arr = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *document = [arr firstObject];
```

#### 12、实例化一个文件句柄并以 只读 的方式打开一个文件

```objcNSFileHandle *fileH = [NSFileHandle fileHandleForReadingAtPath:@"/Users/liuguilin/Desktop/my.txt"];
```

#### 13、以 只写 的方式打开一个文件

```objc
fileH = [NSFileHandle fileHandleForWritingAtPath:@"/Users/liuguilin/Desktop/my.txt"];
```

#### 14、以 读写 的方式打开一个文件

```objc
fileH = [NSFileHandle fileHandleForUpdatingAtPath:@"/Users/liuguilin/Desktop/my.txt"];
```

#### 15、读取文件数据   readDataToEndOfFile从开始一直到最后
    
```objc
NSData *data = [fileH readDataToEndOfFile];
```

#### 16、从文件中读取指定字节的数据 readDataOfLength

```objc
NSData *data1 = [fileH readDataOfLength:10];
```

#### 17、Data类型数据 转换为 NSString类型

```objc
NSString *str = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
```

#### 18、字符串 转化为 NSData类型

```objc
NSData *data = [str dataUsingEncoding:NSUTF8StringEncoding];
```

#### 19、设置读写指针位置 seekToFileOffset

```objc
[fileH seekToFileOffset:29];
// 设置光标到文件最末尾		
[fileH seekToEndOfFile];  
```

#### 20、写入文件 data类型

```objc
[fileH writeData:data];
```

#### 21、字符串写入文件

```objc
NSString *str = @"哎呦喂";
BOOL ret = [str writeToFile:@"/Users/liuguilin/Desktop/1.txt" atomically:YES encoding:NSUTF8StringEncoding error:nil];
```

#### 22、数组写入文件

```objc
NSArray *arr = @[@"1",@"2",@"3"];
BOOL ret = [arr writeToFile:@"/Users/liuguilin/Desktop/1.txt" atomically:YES];
```

#### 23、字典写入文件

```objc
NSDictionary *dic = @{@"one":@"1",@"two":@"2"};
BOOL ret = [dic writeToFile:@"/Users/liuguilin/Desktop/1.txt" atomically:YES];
```

### Json方法调用

#### 24、实例化一个 网络链接

```objc
NSString *str = @"http://10.0.8.8/sns/my/login.php?username=test&password=123456";
NSURL *url = [NSURL URLWithString:str];
```

#### 25、从网络服务器获取NSString 或 NSDate 类型的数据

```objc
NSString *contStr = [NSString stringWithContentsOfURL:url encoding:NSUTF8StringEncoding 
			error:nil];
NSData *data = [NSData dataWithContentsOfURL:url];
```

#### 26、带中文的url 需要UT-8编码  后才可以获取数据

```objc
NSString *str1 = @"https://api.douban.com/v2/book/search?q=ios开发";
str1 = [str1 stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];   //编码后
NSURL *url = [NSURL URLWithString:str1];
```

#### 27、将获取到的数据转化为json格式 直接是根目录了

```objc
NSDictionary *dict = [NSJSONSerialization JSONObjectWithData:data  options:NSJSONReadingMutableContainers error:nil];
```

	
