---
title: iOS实现应用外自带地图、高德地图、百度地图导航
date: 2016-05-19 10:42:54
categories: blog
tags: [Objective-C,地图导航]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/05/19/Map-Guide](http://muhlenxi.com/2016/05/19/Map-Guide)*

### 导语：

> 在地图类应用开发中，我们经常有导航这个功能需求。根据导航方式可以分为应用内导航和应用外导航，其中应用内导航指的是使用第三方提供的地图 SDK（高德、百度等）将导航嵌入到我们开发的 APP 内部。应用外导航指的是以 URL Scheme 跳转的方式，跳转到对应的地图 APP 中，使用对方的导航功能。

  点击阅读全文来了解一下详情吧。

<!-- more -->

本次开发的需求是，实现应用外导航。通过选项列表（`UIAlertController`、`UIActionSheet`）的方式提供用户选择，当用户既安装了高德地图和百度地图时，则弹出如下图所示的选项列表。否则用户安装了哪个地图，就增加哪个地图的选择项。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/actionsheet.PNG" width="320" height="568" alt="选项列表图"/>
</div>

### 环境的配置

在iOS 9 下涉及到平台客户端跳转，系统会自动到项目 `info.plist` 下检测是否设置平台 Scheme，对于需要配置的平台，如果没有配置，将无法正常跳转平台客户端，因此需要配置 Scheme 名单。本文我们需要添加百度地图和高德地图的 scheme 白名单。

具体方法：在项目的 `info.plist` 中添加 `LSApplicationQueriesSchemes` 字段，类型是 Array，然后添加两个 Item。

如图：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/scheme.png" width="475" height="60" alt="选项列表图"/>
</div>

### 根据系统的版本号来初始化对应选项列表

我们需要一个属性来记录导航目标的终点坐标

```objc
@property (nonatomic,assign) CLLocationCoordinate2D coordinate;  //!< 要导航的坐标
```

在 `viewDidLoad` 中初始化该坐标值。

```objc
    //导航到深圳火车站
    self.coordinate = CLLocationCoordinate2DMake(22.53183, 114.117206);
```

我们再定义一个判断系统版本是否大于等于 8.0 的一个宏定义

```objc
//系统版本号是否大于8.0
#define IS_SystemVersionGreaterThanEight  ([UIDevice currentDevice].systemVersion.doubleValue >= 8.0)
```

在低于8.0的系统版本中使用 UIAlertController 会崩溃，所以我们要根据系统版本号来选择合适的选项列表。

在导航按钮的事件响应方法中，我们添加如下代码：

```objc
	 //系统版本高于8.0，使用UIAlertController
    if (IS_SystemVersionGreaterThanEight) {
        
        UIAlertController * alertController = [UIAlertController alertControllerWithTitle:@"导航到设备" message:nil preferredStyle:UIAlertControllerStyleActionSheet];
        //自带地图
        [alertController addAction:[UIAlertAction actionWithTitle:@"自带地图" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            
            NSLog(@"alertController -- 自带地图");
            
            //使用自带地图导航
            MKMapItem *currentLocation =[MKMapItem mapItemForCurrentLocation];
            
            MKMapItem *toLocation = [[MKMapItem alloc] initWithPlacemark:[[MKPlacemark alloc] initWithCoordinate:self.coordinate addressDictionary:nil]];
            
            [MKMapItem openMapsWithItems:@[currentLocation,toLocation] launchOptions:@{MKLaunchOptionsDirectionsModeKey:MKLaunchOptionsDirectionsModeDriving,
                MKLaunchOptionsShowsTrafficKey:[NSNumber numberWithBool:YES]}];
    
            
        }]];
        
        //判断是否安装了高德地图，如果安装了高德地图，则使用高德地图导航
        if ( [[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"iosamap://"]]) {
            
            [alertController addAction:[UIAlertAction actionWithTitle:@"高德地图" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
                
             NSLog(@"alertController -- 高德地图");
             NSString *urlsting =[[NSString stringWithFormat:@"iosamap://navi?sourceApplication= &backScheme= &lat=%f&lon=%f&dev=0&style=2",self.coordinate.latitude,self.coordinate.longitude]stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
                [[UIApplication  sharedApplication]openURL:[NSURL URLWithString:urlsting]];
                
            }]];
        }
        
        //判断是否安装了百度地图，如果安装了百度地图，则使用百度地图导航
        if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"baidumap://"]]) {
            [alertController addAction:[UIAlertAction actionWithTitle:@"百度地图" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
               
             NSLog(@"alertController -- 百度地图");
             NSString *urlsting =[[NSString stringWithFormat:@"baidumap://map/direction?origin={{我的位置}}&destination=latlng:%f,%f|name=目的地&mode=driving&coord_type=gcj02",self.coordinate.latitude,self.coordinate.longitude] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
                [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlsting]];
                
            }]];
        }
        
        //添加取消选项
        [alertController addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
            
            [alertController dismissViewControllerAnimated:YES completion:nil];
            
        }]];
        
        //显示alertController
        [self presentViewController:alertController animated:YES completion:nil];

    }
    else {  //系统版本低于8.0，则使用UIActionSheet
        
        UIActionSheet * actionsheet = [[UIActionSheet alloc] initWithTitle:@"导航到设备" delegate:self cancelButtonTitle:@"取消" destructiveButtonTitle:nil otherButtonTitles:@"自带地图", nil];
        
        //如果安装高德地图，则添加高德地图选项
        if ( [[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"iosamap://"]]) {
            
            [actionsheet addButtonWithTitle:@"高德地图"];
         
        }
        
        //如果安装百度地图，则添加百度地图选项
        if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"baidumap://"]]) {
            
            [actionsheet addButtonWithTitle:@"百度地图"];
        }
    
        [actionsheet showInView:self.view];
       
        
    }

```

### 实现 UIActionSheetDelegate 代理方法

*当使用 `UIActionSheet` 时，需要设置 `Delegate` 为 `self` ,并且遵循 `UIActionSheetDelegate` 协议，实现相应代理方法。*

*当点击取消选项时，会触发该代理方法*

```objc
#pragma mark - UIActionSheetDelegate

- (void)actionSheetCancel:(UIActionSheet *)actionSheet
{
    NSLog(@"ActionSheet - 取消了");
    [actionSheet removeFromSuperview];
}
```
*当点击其他选项是，则会触发下面的代理方法*

*使用自带地图导航时，需要用到 `MKMapItem` ,我们需要导入头文件 `#import <MapKit/MapKit.h>` *

```objc
- (void)actionSheet:(UIActionSheet *)actionSheet clickedButtonAtIndex:(NSInteger)buttonIndex
{
    
    NSLog(@"numberOfButtons == %ld",actionSheet.numberOfButtons);
     NSLog(@"buttonIndex == %ld",buttonIndex);
    
    if (buttonIndex == 0) {
        
        NSLog(@"自带地图触发了");
        
        MKMapItem *currentLocation =[MKMapItem mapItemForCurrentLocation];
        
        MKMapItem *toLocation = [[MKMapItem alloc] initWithPlacemark:[[MKPlacemark alloc] initWithCoordinate:self.coordinate addressDictionary:nil]];
        
        [MKMapItem openMapsWithItems:@[currentLocation,toLocation] launchOptions:@{MKLaunchOptionsDirectionsModeKey:MKLaunchOptionsDirectionsModeDriving,
            MKLaunchOptionsShowsTrafficKey:[NSNumber numberWithBool:YES]}];

    }
    //既安装了高德地图，又安装了百度地图
    if (actionSheet.numberOfButtons == 4) {
        
        if (buttonIndex == 2) {
            
            NSLog(@"高德地图触发了");
            
            NSString *urlsting =[[NSString stringWithFormat:@"iosamap://navi?sourceApplication= &backScheme= &lat=%f&lon=%f&dev=0&style=2",self.coordinate.latitude,self.coordinate.longitude]stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
            [[UIApplication  sharedApplication]openURL:[NSURL URLWithString:urlsting]];
        }
        if (buttonIndex == 3) {
            
            NSLog(@"百度地图触发了");
            NSString *urlsting =[[NSString stringWithFormat:@"baidumap://map/direction?origin={{我的位置}}&destination=latlng:%f,%f|name=目的地&mode=driving&coord_type=gcj02",self.coordinate.latitude,self.coordinate.longitude] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
            [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlsting]];
        }
 
    }
    //安装了高德地图或安装了百度地图
    if (actionSheet.numberOfButtons == 3) {
        
        if (buttonIndex == 2) {
            
            if ( [[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"iosamap://"]]) {
                
                NSLog(@"只安装的高德地图触发了");
                NSString *urlsting =[[NSString stringWithFormat:@"iosamap://navi?sourceApplication= &backScheme= &lat=%f&lon=%f&dev=0&style=2",self.coordinate.latitude,self.coordinate.longitude]stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
                [[UIApplication  sharedApplication]openURL:[NSURL URLWithString:urlsting]];
               
            }
            if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"baidumap://"]]) {
                 NSLog(@"只安装的百度地图触发了");
                NSString *urlsting =[[NSString stringWithFormat:@"baidumap://map/direction?origin={{我的位置}}&destination=latlng:%f,%f|name=目的地&mode=driving&coord_type=gcj02",self.coordinate.latitude,self.coordinate.longitude] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
                [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlsting]];
            }

           
        }
        
    }
    
}

```

### 知识点补充

【1】使用 `canOpenURL` 方法来检测该手机是否安装相应 APP。

*该方法会返回一个 BOOL 值，当为 YES 时，表明已安装该 APP*

```objc
[[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"iosamap://"]]
```
常用的4个地图的 URL Scheme：

* 1、苹果自带地图（不需要检测，所以不需要URL Scheme）
* 2、百度地图 baidumap://
* 3、高德地图 iosamap://
* 4、谷歌地图 comgooglemaps://

[点击这里查看更多常用的URL Scheme](http://www.jianshu.com/p/28f517775214)

[对URL Scheme不了解的点这里](http://sspai.com/31500)

[如何自定义URL Scheme点这里](http://objcio.com/blog/2014/05/21/the-complete-tutorial-on-ios-slash-iphone-custom-url-schemes/)

【2】`wgs84`，`gcj-02`,`bd-09`是什么?

`wgs84`是国际标准，从GPS设备中取出的原始数据就是这个标准的数据，iOS的SDK中用到的坐标系统也是国际标准的坐标系统WGS-84；

`gcj-02`是中国标准，行货GPS设备取出的原始数据是该标准的数据，根据规定，国内出版的各种地图系统，必须至少采用gcj-02对地理位置进行首次加密。网络上也称之为火星坐标。

`bd-09`是百度标准，百度SDK使用的就是这个标准。

[如何进行转换点这里](https://github.com/JackZhouCn/JZLocationConverter)

### 运行结果图示

*以下是选择了不同的选项对应的结果图。*

*自带地图导航。*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/map.PNG" width="320" height="568" alt="自带地图导航"/>
</div>

*高德地图导航。*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/gaodemap.PNG" width="320" height="568" alt="高德地图导航"/>
</div>

*百度地图导航。*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/baidumap.PNG" width="320" height="568" alt="百度地图导航"/>
</div>

*感谢阅读，有什么意见可以给我留言!*