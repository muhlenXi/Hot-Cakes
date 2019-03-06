---
title: 关于 NSURLConnection 的使用
date: 2016-01-15 15:48:39
categories: blog
tags: [NSURLConnection,Objective-C]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/01/15/NSURLConnection](http://muhlenxi.com/2016/01/15/NSURLConnection)*

#### 前言：

> NSURLConnection 是 iOS7之前专门用来做数据请求的类。

学习贵在记录和总结收获！点击阅读全文了解更多！　　

<!-- more -->

#### 基础知识

##### 什么是 HTTP 请求？

*如图就是一个 HTTP 请求简介*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/httpRequest.png" width="646" height="415" alt="选项列表图"/>
</div>

*一个 HTTP 请求包含URL（请求地址）、Method（请求方式）、Header（请求头）、Body（请求体）。*

*一个 HTTP 响应包含Header（响应头）、Body（响应体）。*

##### 什么是 URL？

*URL - 统一资源定位符*

*可以通过 `NSURL class` 来生成一个 URL：*

```objc
NSURL *url = [NSURL URLWithString:@"http://10.0.8.8/sns/my/user_list.php"];
```
##### 什么是 request？

*request - 数据请求*

*可以通过 `NSURLRequest class` 来生成一个 Request：*

创建 Request：

```objc
NSURLRequest *request = [NSURLRequest requestWithURL:url];
```

创建带超时的 Request:

```objc
NSURLRequest *request = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:30.0f]
```

创建可变的的 Request: （可以修改 Request 的属性，如 Method 等，POST 会用到。）

```objc
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
request.HTTPMethod = @"POST";
//设置请求体
NSString *paramStr = @"username=st1508&password=123456";
request.HTTPBody = [paramStr dataUsingEncoding:NSUTF8StringEncoding];
```

#### NSURLConnection 的应用

*NSURLConnection：负责发送请求，建立客户端和服务器的连接。发送 NSURLRequest 的数据给服务器，并收集来自服务器的响应数据。*

发送请求的基本步骤：

发送请求的三个步骤：

* 1.设置请求路径
* 2.创建请求对象
* 3.发送请求（同步或异步请求）

> 发送同步请求（一直在等待服务器返回数据，这行代码会卡住，如果服务器，没有返回数据，那么在主线程UI会卡住不能继续执行操作）有返回值
> 
> 发送异步请求：没有返回值
> 
> 注意：任何 NSURLRequest 默认都是 Get 请求。

#### NSURLConnection 代码举例

##### 发送一个同步请求:

```objc
NSString *urlStr = @"请求的地址";
NSURL * URL = [NSURL URLWithString:urlStr];
NSURLRequest * request = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:30.0];  //超时30s
NSError * error = nil;  //请求错误
NSURLResponse *response = nil;   //请求响应
//请求响应的数据
NSData * data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&error];
//请求成功
if (error == nil ) {
    NSDictionary * jsonDic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableLeaves error:nil];
        
    NSLog(@"jsonDic == %@",jsonDic);
    //在这里处理响应的数据
			
}
else
{
    NSLog(@"请求失败：%@",error.localizedDescription);
	//在这里处理请求失败的情况
}
```

##### 发送一个异步请求: 默认是 Get

*方式一：请求的结果通过 Block 返回。*

```objc
NSURL *URL = [NSURL URLWithString:@"请求的地址"];
    NSURLRequest *request = [NSURLRequest requestWithURL:URL];
    
// 发送一个异步请求，请求的结果是通过completionHandler这个Block返回的
[NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
    if (connectionError != nil) {
         NSLog(@"请求出错: %@", connectionError);
         //在这里处理请求失败的情况
    }
    else {
         NSLog(@"请求成功，解析数据");
            
         id jsonObj = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
         NSLog(@"jsonObj == %@",jsonObj);
         //在这里处理响应的数据    
    }
}];

```

*方式二：请求的结果通过 Delegate 返回。*

*详情请看 [NSURLConnectionWithDelegateDemo](https://github.com/YinjunXi/NSURLConnectionWithDelegateDemo)。*

##### 发送一个 POST 异步请求: 

```objc
NSURL *url = [NSURL URLWithString:@"http://10.0.8.8/sns/my/user_list.php"];
    
// NSMutableURLRequest 可以修改请求的一些默认设置
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
    

// 如果有必要，可以设置请求头
// [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
// [request setValue:@"2555" forHTTPHeaderField:@"Content-Length"];

    
// 把请求的Method改成POST，默认是GET
// GET请求的参数，是拼在URL的后面
// POST请求参数，是放在请求体里面的
request.HTTPMethod = @"POST";
    
// page=1&number=5 默认
NSString *paramStr = @"page=1&number=2";
// 把字符串转成二进制对象，NSString -> NSData
NSData *paramData = [paramStr dataUsingEncoding:NSUTF8StringEncoding];
    
// 把二进制对象，转成字符串，NSData -> NSString
// [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
    
// 把参数放到请求体里面
request.HTTPBody = paramData;
    
// 如果接口要求参数是一个JSON，就这样写，在这里没办法测试
// NSDictionary *dict = @{@"page":@"1", @"number": @"5"};
// NSData *paramData2 =
[NSJSONSerialization dataWithJSONObject:dict options:NSJSONWritingPrettyPrinted error:nil];
// request.HTTPBody = paramData;

// 发送POST的异步请求
[NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
    if (connectionError == nil) {
        id jsonObj = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
         NSLog(@"jsonObj == %@",jsonObj);
         //在这里处理响应的数据
         
    } else {
         NSLog(@"请求失败: %@", connectionError);
   }
}];
```

*感谢您的阅读，一起学习，一起成长，加油！*
