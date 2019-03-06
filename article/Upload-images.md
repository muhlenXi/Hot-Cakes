---
title: iOS用户头像的获取与上传
date: 2015-12-01 10:03:45
categories: blog
tags: [Objective-C,头像上传]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2015/12/01/Upload-images](http://muhlenxi.com/2015/12/01/Upload-images)*

### 导语：

> 在 APP 的开发过程中，经常涉及到个人信息界面的开发，其中有一个常见的功能，就是用户头像的选择与上传。用户可以通过拍照或者在相册中选择一个已经存在的照片，然后设置为头像并上传到服务器中储存。我们来看看这个功能是怎么实现的。作为一个程序猿，生命不止，学习则不止。

  点击阅读全文来了解一下详情吧。

<!-- more -->

本次开发的需求是，实现一个个人信息界面，用户可以通过两种不同的方式来选择头像并保存到服务器中。本次开发是通过 `AFNetworking` 这个框架来实现头像的上传功能，通过 `SDWebImage` 这个框架来实现图片的异步下载和缓存。

[AFNetworking 传送门](https://github.com/AFNetworking/AFNetworking)

[SDWebImage 传送梦](https://github.com/rs/SDWebImage)

类似于下面这样的界面：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/myself.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*UI界面的搭建这里就忽略了。接下来开始办大事了*

### 选项表的实现

*不会选项表的可以看这里，记得要设置代理和遵循 'UIActionSheetDelegate' 协议啊,在需要弹出选项列表的事件响应方法中写入如下代码即可*

```objc
UIActionSheet * actionSheet = [[UIActionSheet alloc] initWithTitle:nil delegate:self cancelButtonTitle:@“取消” destructiveButtonTitle:nil otherButtonTitles:@“拍照”,@“从手机相册中选择”, nil];

[actionSheet showInView:self.view];
```
*接下来实现代理方法*

```objc
#pragma mark - UIActionSheetDelegate

- (void)actionSheet:(UIActionSheet *)actionSheet clickedButtonAtIndex:(NSInteger)buttonIndex
{
    //NSLog(@"buttonIndex == %ld",buttonIndex);
    switch (buttonIndex) {
        case 0:  //照相机
        {
            [self presentCameraImagePicker];
        }
            break;
        case 1:  //从相册获取
        {
            [self presentPhotoLibraryImagePicker];
        }
            break;
        case 2:
        {
            NSLog(@"取消了头像选择");
        }
            break;
        default:
            break;
    }
}

```

这里用到的方法，下面会详细介绍。

### 工欲善其事必先利其器

*这里详细介绍前面调用的的几个方法*

`拍照`和`从手机相册中选择`功能需要用到`UIImagePickerController`,我们就从这个开始写起。

*记得遵循 `UIImagePickerControllerDelegate,UINavigationControllerDelegate` 协议。*

#### 拍照

【1】拍照

```objc
- (void) presentCameraImagePicker
{
    //首先判断该设备是否支持照相功能
    if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
        
        UIImagePickerController * imagePicker = [[UIImagePickerController alloc] init];
        imagePicker.delegate = self;
        imagePicker.allowsEditing = YES;
        
        //设置为图片的来源为拍照获取模式
        imagePicker.sourceType = UIImagePickerControllerSourceTypeCamera;
        
        imagePicker.modalPresentationStyle = UIModalPresentationCurrentContext;
        
        [self  presentViewController:imagePicker animated:YES completion:nil];
        
        
    } else {
        NSLog(@"该设备无摄像头");
    }
    
}
```

#### 从手机相册中选择照片

【2】从手机相册中选择照片

```objc
- (void) presentPhotoLibraryImagePicker
{
    UIImagePickerController * imagePicker = [[UIImagePickerController alloc] init];
    imagePicker.delegate = self;
    imagePicker.allowsEditing = YES;
    
    //选择图片的来源为从相册获取
    imagePicker.sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
    [self presentViewController:imagePicker animated:YES completion:nil];
}
```

#### 保存照片到本地沙盒中

【3】保存照片到本地沙盒中，并根据条件判断是否要上传到服务器中

```objc
- (void) saveImage:(UIImage *) image  NeedUpload:(BOOL) upload
{
    NSFileManager * fileManager = [NSFileManager defaultManager];
    NSError * error = nil;
    
    //照片的存储路径
    NSString * imagePath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES)[0];
    //添加照片的名称
    imagePath = [imagePath stringByAppendingPathComponent:@"headImage.png"];
    NSLog(@"imagePath == %@",imagePath);
    
    //如果本地存在该照片则删除
    if ([fileManager fileExistsAtPath:imagePath] == YES) {
        BOOL ret = [fileManager removeItemAtPath:imagePath error:&error];
        if (ret) {
            NSLog(@"该路径存在该照片，已删除该文件");
        }   
    }
    
    //根据项目要求实际需求改变照片的大小
    //改变图像尺寸为120*120
    CGSize size = CGSizeMake(120, 120);
    UIGraphicsBeginImageContext(size);
    [image drawInRect:CGRectMake(0, 0, size.width, size.height)];
    UIImage * newImage = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    //将图片写入本地沙盒中
    BOOL ret = [UIImageJPEGRepresentation(newImage, 1.0) writeToFile:imagePath atomically:YES];
    if (ret == YES) {
        NSLog(@"保存头像图片到本地成功");
        
        if (upload == YES) {
            //上传头像到服务器
            [self uploadHeadImageToServer];
        }  
    } else {
        NSLog(@"保存头像图片到本地失败");
    }  
}
```
#### 通过 AFNetworking 上传到服务器中

【4】关键的一步，上传到服务器中

```objc
- (void) uploadHeadImageToServer
{
    //以下的URL和参数需要根据项目的接口文档做调整
        
    NSString * url = @“这里填写上传头像的URLString”;
    
    NSDictionary * dic2 = @“这里写具体的上传参数”;
        
    NSLog(@"上传头像Parameters == %@",dic2);
    
    //初始化AFHTTPSessionManager
    AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
    // 设置二进制响应解析器
    manager.responseSerializer = [AFHTTPResponseSerializer serializer];
    // 设置二进制请求解析器 
    manager.requestSerializer = [AFJSONRequestSerializer serializer];
    // 设置超时时间
    [manager.requestSerializer willChangeValueForKey:@"timeoutInterval"];
    manager.requestSerializer.timeoutInterval = 10.f;
    [manager.requestSerializer didChangeValueForKey:@"timeoutInterval"];
    
    // AFNetworking进行multipart/form-data的请求方法
    // constructingBodyWithBlock 是用来构造请求体的(HTTPBody)
    
    //Post请求上传数据
    [manager POST:url parameters:dic2 constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
        
        // 添加照片的文件流
        NSError *error = nil;
        NSString * imagePath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES)[0];
        imagePath = [imagePath stringByAppendingPathComponent:@"headImage.png"];
        [formData appendPartWithFileURL:[NSURL fileURLWithPath:imagePath] name:@"headImage" error:&error];
        if (error != nil) {
            NSLog(@"添加文件流出错: %@", error);
        }else {
            NSLog(@"添加文件流成功");
        } 
        
    } progress:^(NSProgress * _Nonnull uploadProgress) {
        
        
    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
        
        NSLog(@"上传头像数据请求成功");
        id jsonObj = [NSJSONSerialization JSONObjectWithData:responseObject options:NSJSONReadingMutableContainers error:nil];
        NSLog(@"%@", jsonObj);
        
        //这里做数据的额后续处理，需根据实际需求操作
        NSLog(@"msg == %@",jsonObj[@"msg"]);
        NSDictionary * result = jsonObj[@"result"];
        
        //拼接图片网络URL
        NSString * headImageURL = [NSString stringWithFormat:@"%@/%@",FF_doname_uploadImage,result[@"headImage"]];
        NSLog(@"headImageURL == %@",headImageURL);
        
        //将头像的网络URL保存到本地
        [[NSUserDefaults standardUserDefaults] setObject:headImageURL forKey:@"headImageURL"];
        [[NSUserDefaults standardUserDefaults] synchronize];
        
        //设置头像
        [self.headImageView sd_setImageWithURL:[NSURL URLWithString:headImageURL] placeholderImage:[UIImage imageNamed:@"head60"]];
        
    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
         NSLog(@"上传头像数据请求成功数据请求失败：%@", error);
    }];   
}
```

### 拍照或选择图片的事件处理

* 当拍照后，点击`使用照片`会触发这个代理方法。

* 当从相册中选择照片后，点击`选取`后会触发下面这个代理方法，在这个方法中，我们可以处理获取到的照片。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/choose.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*触发的代理方法如下*

```objc
#pragma mark - UIImagePickerControllerDelegate

- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info
{
    [picker dismissViewControllerAnimated:YES completion:nil];
    
    //获取图片
    UIImage * image = [info objectForKey:UIImagePickerControllerEditedImage];
    
    [self saveImage:image NeedUpload:YES];  //保存图片到本地,并上传到服务器
    
}
```

*点击取消会触发这个方法，在这个方法中，我们要关闭照相页面*

```objc
- (void)imagePickerControllerDidCancel:(UIImagePickerController *)picker
{
    [picker dismissViewControllerAnimated:YES completion:nil];
}
```

编译运行代码，如果没问题的话，效果如下:

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/finally.PNG" width="320" height="568" alt="选项列表图"/>
</div>

是不是难度不是很大，恭喜您又学会一招，其实在开发过程中，看优秀的开源代码能然我们走的更远！

### 上传多张照片到服务器：

*AFNetworking 3.0+ 上传多张图片*

```objc
//上传多张照片到服务器
- (void) uploadPhotosToTheServer
{
    
    // 1、设置上传图片的接口路径
    NSString *urlString = @"上传图片的地址";
    
    NSMutableDictionary * paramDic = [NSMutableDictionary dictionary];
    // 2、配置上传参数
    //比如：paramDic[@"date"] = @"2016-11-11";

    // 基于AFN3.0+ 封装的HTPPSession句柄
    AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
    manager.requestSerializer.timeoutInterval = 200;
    manager.responseSerializer.acceptableContentTypes = [NSSet setWithObjects:@"text/plain", @"multipart/form-data", @"application/json", @"text/html", @"image/jpeg", @"image/png", @"application/octet-stream", @"text/json", nil];
    
 
    [manager POST:urlString parameters:paramDic constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
        
        // formData: 专门用于拼接需要上传的数据,在此位置生成一个要上传的数据体
        // self.photoArr：是你存放图片的数组,可以是图片，可以是保存在本地的图片路径
        
        // 3、循环添加图片的数据
        for (int i = 0; i < self.photoArr.count; i++) {
            
            UIImage *image = [UIImage imageWithContentsOfFile:self.self.photoArr[i]];
            NSData *imageData = UIImageJPEGRepresentation(image, 0.5);
            
            // 在网络开发中，上传文件时，是文件不允许被覆盖，文件重名
            // 要解决此问题，
            // 可以在上传时使用当前的系统事件时间字符串作为文件名
            NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
            // 设置时间格式
            [formatter setDateFormat:@"yyyyMMddHHmmss"];
            NSString *dateString = [formatter stringFromDate:[NSDate date]];
            NSString *fileName = [NSString  stringWithFormat:@"%@.jpg", dateString];
            
            /*
             *该方法的参数
             1. appendPartWithFileData：要上传的照片[二进制流]
             2. name：对应网站上[upload.php中]处理文件的字段（比如upload）
             3. fileName：要保存在服务器上的文件名
             4. mimeType：上传的文件的类型
             */
            [formData appendPartWithFileData:imageData name:@"upload" fileName:fileName mimeType:@"image/jpeg"]; //
        }
        
    } progress:^(NSProgress * _Nonnull uploadProgress) {
        
        NSLog(@"---上传进度--- %@",uploadProgress);
        
    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
        
        NSLog(@"---上传成功--- %@",responseObject);
        
        
    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
        
        NSLog(@"---上传失败--- %@", error);
        
    }];
}
```

*感谢阅读，有什么意见可以给我留言!*