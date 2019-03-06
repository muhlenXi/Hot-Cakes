---
title: ASIHTTPRequest 的简介和使用
date: 2015-08-15 14:08:37
categories: blog
tags: [Objective-C,ASIHttpRequest]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2015/08/15/ASIHTTPRequest](http://muhlenxi.com/2015/08/15/ASIHTTPRequest)*

### 导语：

> ASIHttpRequest 是一款及其强劲的 HTTP 访问开源项目，让简单的 API 完成复杂的功能，如：异步请求、队列请求、GZIP 压缩、缓存、断点续传、进度跟踪、上传文件、HTTP 认证，同时加入了 Objective-C 闭包 Block 的支持，让我们的代码更加灵活。

　　由于 ASIHTTPRequest 是开源项目旧版本的库，没有适配 ARC 项目。所以对于目前之前 ARC 的工程来说，需要做适配。
　　
　　<!-- more -->


### ASIHTTPRequest 的环境配置

#### 下载 ASIHTTPRequest 库

　　点击[ASIHTTPRequest](https://github.com/paytronix/ASIHTTPRequest)下载ASIHTTPResent库。

#### 添加 ASIHTTPRequest 库 到项目组中

　　找到 `ASIHTTPRequest-master` 目录，将目录中的 `ASIHTTPRequest` 文件夹内的所有文件添加到项目中。

#### 添加 系统依赖库

【1】选择工程 -> `TARGETS` -> `Building Phases` -> `Link Binary With Libraries`

【2】点击 “`+`” 号搜索添加。

    1、CFNetwork.framework
    2、SystemConfiguration.framework
    3、MobileCoreServices.framework
    4、CoreGraphics.framework
    5、libz.1.2.5.tbd

【3】编译运行，会报错。这是ARC使用非ARC库导致的。所以要适配非ARC库，也就是说要在费ARC文件中添加 `-fno-objc-arc` 字段

####  ARC 环境下适配非 ARC 文件

【1】选择工程 -> `TARGETS` -> `Building Phases` -> `Compile Source`

【2】双击其中的 `.m` 文件，添加 `-fno-objc-arc` 字段。

 　　适配完如图所示：
 
<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/asihttprequest.png" width="765" height="634" alt="选项列表图"/>
</div> 
 
### ASIHTTPRequest 的使用

#### 异步网络请求

【1】导入头文件

    #import "ASIHTTPRequest.h"
    
【2】创建并启动异步网络请求

    NSString * urlstr = @"http://app.careeach.com:80/action/json_201411/userdata.jsp?action=userdata&userid=22115195&waibao_id=0";
    
    NSURL * url = [NSURL URLWithString:urlstr];
    
    ASIHTTPRequest * request = [ASIHTTPRequest requestWithURL:url];
    
    request.delegate = self;
    
    [request startAsynchronous];

【3】实现请求回调方法

    #pragma mark - ASIHTTPRequest-delegate

    - (void)requestFinished:(ASIHTTPRequest *)request
    {
        NSString *respondStr = [request responseString];
    
        NSLog(@"获取个人信息成功");
    
        if(respondStr != nil)
        {
            NSError *error;
            NSData *data = [respondStr dataUsingEncoding:NSUTF8StringEncoding];
           NSDictionary * infoDic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableLeaves error:&error];
        
            NSLog(@"inforDic == %@",infoDic);
        }
 
    }

    - (void) requestFailed:(ASIHTTPRequest *)request
    {
        NSLog(@"获取个人信息错误,error = %@",request.error);
    }
 
　　编译并运行项目后，结果如下：

![result](http://7xvffo.com1.z0.glb.clouddn.com/asireqult.png)  
 
　　*未使用到的会后续补充...,谢谢！*



