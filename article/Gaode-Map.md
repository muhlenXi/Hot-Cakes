---
title: 高德 SDK 的正确打开方式
date: 2016-04-07 10:02:20
categories: blog
tags: [Objective-C,高德地图SDK]
---
 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/04/07/Gaode-Map](http://muhlenxi.com/2016/04/07/Gaode-Map)*

### 导语：

> 高德地图 iOS SDK 是一套基于 iOS 6.0.0 及以上版本的地图应用程序开发接口，供开发者在自己的 iOS应用中加入地图相关的功能。通过 iOS SDK，开发者可以轻松地开发出地图显示与操作、兴趣点搜索、地理编码、路线规划等功能。

*点击下载[本文的demo](https://github.com/muhlenXi/gaodeMapDemo)研究，记得给star支持博主的努力哦！*


  点击阅读全文来瞅瞅如何开发吧。

<!-- more -->

*由于官方文档更新不及时，很多方法弃用了，新的方法只能自己摸索了。本文大部分是参考官方文档来做的。*

点这里查看[高德开发平台](http://lbs.amap.com),在这里注册`高德账号`并成为`开发者`。

*如果是第一次接触高德地图，可以大概看一下[入门指南](http://lbs.amap.com/api/ios-sdk/gettingstarted/)*

### 高德地图SDK的部署

*由于我喜欢简单粗暴的部署方式，所以一直选择 `CocoaPods` 来安装和配置第三方库*。

关于 `CocoaPods `,不了解的可以看一下我的另一篇博文，[zsh的配置和CocoaPods的安装与使用](http://muhlenxi.com/2015/10/05/zsh-and-CocoaPods)。这里就不详细展开了。

方法就是，使用 `终端cd` 到你的项目所在的路径下，`vim` 一个 `Podfile` 文件，点击 `字母i` 进入`编辑`状态，在文件中输入：

```objc
platform:ios,'7.0'
target 'gaodeMapDemo' do
pod 'AMap2DMap', '~> 3.3.0'
pod 'AMapLocation', '~> 1.2.0' #定位SDK
pod 'AMapSearch'               #搜索服务SDK
end
```

然后按下 `Esc`，`shift+：`然后输入 `wq` ，最后 `终端` 输入 `pod install --verbose --no-repo-update` 安装即可。

### 高德地图SDK的打开方式

点这里查看[高德地图 iOS SDK](http://lbs.amap.com/api/ios-sdk/summary/)。

配置好就可以正式干活了，我们先导入高德地图的头文件 `#import <MAMapKit/MAMapKit.h>`

先来一个屏幕宽和高的宏定义吧。

```objc
#define kScreenWidth [UIScreen mainScreen].bounds.size.width
#define kScreenHeight [UIScreen mainScreen].bounds.size.height
```

再`声明`一个 `mapView` 的属性。

```objc
@property (nonatomic,strong) MAMapView * mapView;
```

#### 添加地图

自定义一个设置地图的方法 `setupMap`

```objc
//设置高德地图
- (void) setupMap
{
    [MAMapServices sharedServices].apiKey = gaode_key;
    
    self.mapView = [[MAMapView alloc] initWithFrame:CGRectMake(0, 0, kScreenWidth, kScreenHeight)];
    [self.view addSubview:self.mapView];
    
    //设置地图模式
    // MAMapTypeSatellite  // 卫星地图
    // MAMapTypeStandard   // 普通地图
    self.mapView.mapType = MAMapTypeStandard;
    
}
```

在 `viewDidLoad` 中调用这个方法，然后 `编译运行`，看看添加的地图长啥样。

会发现基本长这样：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/gaodebiaozhun.PNG" width="320" height="568" alt="选项列表图"/>
</div>

`卫星地图` 基本长这样：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/gaodeweixing.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*显示实时路况，默认是不显示的*

```objc
self.mapView.showTraffic = YES;
```

*不显示比例尺，默认是显示的,可以通过 `compassOrigin` 属性可改变指南针的显示位置。*

```objc
self.mapView.showsScale = NO;
//设置比例尺位置
self.mapView.scaleOrigin= CGPointMake(_mapView.scaleOrigin.x, 82);
```

*不显示罗盘，默认是显示的,可以通过 `scaleOrigin` 属性可改变改变比例尺的显示位置*

```objc
self.mapView.showsCompass = NO;
//设置指南针位置
self.mapView.compassOrigin= CGPointMake(_mapView.compassOrigin.x, 82);
```
*高德地图的 Logo 不能移除，可以通过 logoCenter 属性来调整 Logo 的显示位置。*

```objc
//显示logo在右下角
self.mapView.logoCenter = CGPointMake(kScreenWidth-55, kScreenHeight-13);
```



#### 设置地图的中心坐标和显示范围

点这里[坐标拾取器](http://lbs.amap.com/console/show/picker)来获取经纬度吧。

*写一个根据经纬度和跨度来设置地图显示范围的方法*

```objc
- (void) setMapViewDisplayAreaWithLatitude:(double)lati andLongitude:(double) longi Span:(double) span_value
{
    CLLocationCoordinate2D center = {lati,longi};
    
    MACoordinateSpan span = {span_value,span_value};
    
    MACoordinateRegion region = {center,span};
    
    [self.mapView setRegion:region animated:YES];
}
```

在 `viewDidLoad` 中调用这个方法。

```objc
	//设置显示区域为深圳展滔科技大厦114.038072,22.639641
    double lati = 22.639641;
    double longi = 114.038072;
    [self setMapViewDisplayAreaWithLatitude:lati andLongitude:longi Span:0.02];
```
然后`编译运行`，结果如下。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/gaodezhantao.PNG" width="320" height="568" alt="选项列表图"/>
</div>

#### 添加系统样式的标注

*标注可以精确表示用户需要展示的位置信息，高德地图SDK提供的标注功能允许用户自定义图标和信息窗，同时提供了标注的点击、拖动事件的回调。*

*写一个根据经纬度添加系统样式标注的方法*

```objc
//添加默认样式的点标记，即大头针
- (void) addPointAnnotationWithLatitude:(double)lati andLongitude:(double) longi Title:(NSString *) title Subtitle:(NSString *) subtitle
{
    MAPointAnnotation * pointAnnotation = [[MAPointAnnotation alloc] init];
    pointAnnotation.coordinate = CLLocationCoordinate2DMake(lati, longi);
    pointAnnotation.title = title;
    pointAnnotation.subtitle = subtitle;
    
    [self.mapView addAnnotation:pointAnnotation];
}
```

在 `viewDidLoad` 中添加标注

```objc
//添加系统样式标注
[self addPointAnnotationWithLatitude:lati andLongitude:longi Title:@"展滔科技大厦" Subtitle:@"深圳市宝安区民治大道1079号"];
```

**此时编译运行的话，地图上仅仅显示一个红色的大头针，点击大头针没任何反应**

我们在软件开发中，设计到`协议`、`代理`、`回调方法`时，一定要切记这三步曲！

* 第一步：遵循协议。
* 第二步：设置代理（这步千万不能忘）！
* 第三步：实现相应代理方法。

第一步遵循 `<MAMapViewDelegate>` 协议。

```objc
@interface RootViewController ()<MAMapViewDelegate>
```

第二步设置代理，在 `setupMap` 方法中加入如下代码。

```objc
//设置地图的代理
self.mapView.delegate = self;
```

第三步通过实现协议中的 `mapView:viewForAnnotation:` 回调函数，设置标注样式.

```objc
#pragma mark - MAMapViewDelegate

- (MAAnnotationView *)mapView:(MAMapView *)mapView viewForAnnotation:(id<MAAnnotation>)annotation
{
    if ([annotation isKindOfClass:[MAPointAnnotation class]])
    {
        //复用标识符
        static NSString * pointReuseIndentifier = @"pointReuseIndentifier";
        MAPinAnnotationView * annotationView = (MAPinAnnotationView*)[mapView dequeueReusableAnnotationViewWithIdentifier:pointReuseIndentifier];
        if (annotationView == nil)
        {
            annotationView = [[MAPinAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:pointReuseIndentifier];
        }
        //设置气泡可以弹出，默认为NO
        annotationView.canShowCallout= YES;
        //设置标注动画显示，默认为NO
        annotationView.animatesDrop = YES;
        //设置标注可以拖动，默认为NO
        annotationView.draggable = YES;
        //设置大头针的颜色，有MAPinAnnotationColorRed, MAPinAnnotationColorGreen, MAPinAnnotationColorPurple三种
        annotationView.pinColor = MAPinAnnotationColorPurple;
        
        return annotationView;
    }
    return nil;
}
```

*此时编译并运行代码，地图上会显示一个紫色的大头针，点击大头针会弹出一个气泡，气泡中显示设置的标题和副标题。如图所示：*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/datouzhen.PNG" width="320" height="568" alt="选项列表图"/>
</div>


*添加一组标注用这个方法*

```objc
//数组中存放的是MAPointAnnotation的对象
NSArray * array = @[pointAnnotation];
[self.mapView addAnnotations:array];
```

*删除一个标注*

```objc
[self.mapView removeAnnotation:pointAnnotation];
```
*删除一组标注*

```objc
//数组中存放的是要删除的标注，也是MAPointAnnotation的对象
NSArray * array = @[pointAnnotation];   
[self.mapView removeAnnotations:array];
```

#### 添加自定义样式的标注

自定义标注的效果如图所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/zidingyibiaozhu.PNG" width="320" height="568" alt="选项列表图"/>
</div>

如果想实现这个效果，我们需要自定义两个 View。

* 1、CustomCalloutView : UIView  //自定义的气泡
* 2、CustomAnnotationView : MAAnnotationView  //自定义的标注View

##### 新建我们的CustomCalloutView：

在 `CustomCalloutView.h` 文件中声明我们需要的属性：

```objc
@property (nonatomic,strong) UIImage * thumbnail;  //!< 缩略图片
@property (nonatomic,copy) NSString * title;  //!< 标题
@property (nonatomic,copy) NSString * address; //!< 地址
```

在 `CustomCalloutView.m` 文件中 `新建一个匿名类（类扩展）` 声明我们需要的控件：

```objc
@interface CustomCalloutView ()

@property (nonatomic,strong) UIImageView * thumbnailImageView;  //!< 缩略图
@property (nonatomic,strong) UILabel * titleLabel;  //!< 标题Label
@property (nonatomic,strong) UILabel * addressLabel;  //!< 地址Label


@end
```

重写 `- (instancetype)initWithFrame:(CGRect)frame` 方法，初始化我们的控件

```objc
- (instancetype)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        
        //其他控件的初始化写在这里
        self.backgroundColor = [UIColor clearColor];
        
        //初始化缩略图
        self.thumbnailImageView = [[UIImageView alloc] initWithFrame:CGRectMake(5, 5, 70, 50)];
        self.thumbnailImageView.backgroundColor =  [UIColor yellowColor];
        [self addSubview:self.thumbnailImageView];
        
        //初始化标题Label
        self.titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(5+70+5, 5, 120-8, 20)];
        self.titleLabel.font = [UIFont systemFontOfSize:14];
        self.titleLabel.textColor =  [UIColor whiteColor];
        self.titleLabel.text = @"title";
        [self addSubview:self.titleLabel];
        
        //初始化地址Label
        self.addressLabel = [[UILabel alloc] initWithFrame:CGRectMake(5+70+5, 20, 120-8, 20*2)];
        self.addressLabel.font = [UIFont systemFontOfSize:12];
        self.addressLabel.numberOfLines = 0;
        self.addressLabel.textColor =  [UIColor lightGrayColor];
        self.addressLabel.text = @"address";
        [self addSubview:self.addressLabel];
        
        
    }
    return self;
}

```

重写 `CustomCalloutView.h` 文件中声明的属性的 `Setter` 方法，方便我们给控件赋值：

```objc
- (void)setTitle:(NSString *)title
{
    self.titleLabel.text = title;
}

- (void)setAddress:(NSString *)address
{
    self.addressLabel.text = address;
}

- (void)setThumbnail:(UIImage *)thumbnail
{
    self.thumbnailImageView.image = thumbnail;
}
```

最后重写 `- (void)drawRect:(CGRect)rect` 方法绘制我们气泡的背景和添加阴影效果：

```objc
- (void)drawRect:(CGRect)rect
{
    //绘制曲线
    CGContextRef contextRef = UIGraphicsGetCurrentContext();
    CGContextSetLineWidth(contextRef, 2.0f);
    CGContextSetFillColorWithColor(contextRef, [UIColor colorWithRed:0.3 green:0.3 blue:0.3 alpha:0.8].CGColor);
    
    CGRect calloutRect = self.bounds;
    CGFloat radius = 6.0;
    CGFloat triangleHeight = 10;   //倒三角的高度
    
    CGFloat x = CGRectGetMinX(calloutRect);
    CGFloat midX = CGRectGetMidX(calloutRect);
    CGFloat width = CGRectGetMaxX(calloutRect);
    
    CGFloat y = CGRectGetMinY(calloutRect);
    CGFloat height = CGRectGetMaxY(calloutRect)-10;
    
    //用起始点开始
    CGContextMoveToPoint(contextRef, x+radius, y);
    
    //画上面的直线
    CGContextAddLineToPoint(contextRef, x+width-radius, y);
    //画右上角圆弧
    CGContextAddArcToPoint(contextRef, x+width, y, x+width, y+radius, radius);
    
    //画右边的直线
    CGContextAddLineToPoint(contextRef, x+width, y+height-radius);
    //画右下角弧线
    CGContextAddArcToPoint(contextRef, x+width, y+height, x+width-radius, y+height, radius);
    
    //画倒三角靠右的下边的直线
    CGContextAddLineToPoint(contextRef, midX+triangleHeight, y+height);
    
    //画倒三角
    CGContextAddLineToPoint(contextRef, midX, y+height+triangleHeight);
    CGContextAddLineToPoint(contextRef, midX-triangleHeight, y+height);
    //画倒三角靠左的下边的直线
    CGContextAddLineToPoint(contextRef, x+radius, y+height);
    
    //画左下角的弧线
    CGContextAddArcToPoint(contextRef, x, y+height, x, y+height-radius, radius);
    
    //画左边的直线
    CGContextAddLineToPoint(contextRef, x, y+radius);
    //画左上角的弧线
    CGContextAddArcToPoint(contextRef, x, y, x+radius, y, radius);
        
    CGContextClosePath(contextRef);
    
    CGContextFillPath(contextRef);
    
    //添加阴影
    self.layer.shadowColor = [[UIColor blackColor] CGColor];
    //阴影的透明度
    self.layer.shadowOpacity = 1.0f;
    //阴影的偏移
    self.layer.shadowOffset = CGSizeMake(0, 0);
}
```

*到这里我们的自定义气泡就大功告成了！*

##### 新建我们的CustomAnnotationView：

新建一个继承自 `MAAnnotationView` 的CustomAnnotationView，在  `CustomAnnotationView.h`文件中，导入前面的自定义气泡的头文件：

    #import "CustomCalloutView.h"
    
声明一个气泡的属性：

```objc
@property (nonatomic,strong) CustomCalloutView * calloutView;
```

在 `CustomAnnotationView.m` 文件中重写标注选中的方法 `- (void)setSelected:(BOOL)selected animated:(BOOL)animated`:

```objc
//重写选中方法setSelected
- (void)setSelected:(BOOL)selected animated:(BOOL)animated
{
    if (self.selected == selected) {
        return;
    }
    if (selected == YES) {
        //如果气泡为空则初始化气泡
        if (self.calloutView == nil) {
            
            self.calloutView = [[CustomCalloutView alloc] initWithFrame:CGRectMake(0, 0, 200, 70)];
            
            //设置气泡的中心点
            CGFloat center_x = self.bounds.size.width/2;
            CGFloat center_y = -self.bounds.size.height;
            self.calloutView.center = CGPointMake(center_x, center_y);
        }
        
        //给calloutView赋值
        self.calloutView.thumbnail = [UIImage imageNamed:@"zhantao"];
        self.calloutView.title = self.annotation.title;
        self.calloutView.address = self.annotation.subtitle;
        
        [self addSubview:self.calloutView];
        
    } else {
        //移除气泡
        [self.calloutView removeFromSuperview];
    }
    
    [super setSelected:selected animated:animated];
}
```

*到这里我们的自定义标注View也完成了，接下来就能使用了*

在我们的地图的标注回调函数 `mapView: viewForAnnotation:` 中设置我们的CustomAnnotationView：

```objc
- (MAAnnotationView *)mapView:(MAMapView *)mapView viewForAnnotation:(id<MAAnnotation>)annotation
{
    if ([annotation isKindOfClass:[MAPointAnnotation class]])
    {
        
        static NSString * reuseIndetifier = @"annotationReuseIndetifier";
        CustomAnnotationView * annotationView = (CustomAnnotationView *)[mapView dequeueReusableAnnotationViewWithIdentifier:reuseIndetifier];
        if (annotationView == nil)
        {
            
            annotationView = [[CustomAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:reuseIndetifier];
            
            annotationView.image = [UIImage imageNamed:@"dingfen"];
            
            ///设置中心点偏移，使得标注底部中间点成为经纬度对应点
            annotationView.centerOffset = CGPointMake(0, -15);
            
            return annotationView;
        }
        
    }
    return nil;
}
```

*如果没问题的话，运行的结果会和前面提到的效果图一致的！*

#### 标注移动的回调处理

*当我们`长按住大头针`时会移动大头针在地图上的位置。高德地图会回调如下方法*

```objc
//移动标注会调用这个方法
- (void)mapView:(MAMapView *)mapView annotationView:(MAAnnotationView *)view didChangeDragState:(MAAnnotationViewDragState)newState fromOldState:(MAAnnotationViewDragState)oldState
{
    //标注停止移动时
    if (newState == MAAnnotationViewDragStateEnding)
    {
        
        //获取标注新的位置所在的经纬度坐标
        CGPoint endPoint = view.centerOffset;
        CLLocationCoordinate2D coo =   [mapView convertPoint:endPoint toCoordinateFromView:view];
        
        NSLog(@"移动后大头针的经纬度：latitude = %f longitude = %f",coo.latitude,coo.longitude);
        
    }
}
```

在地图上移动大头针后，会输出如下结果：

    2016-09-27 16:49:35.255 gaodeMapDemo[5930:2794113] 移动后大头针的经纬度：latitude = 22.646707 longitude = 114.035228


#### 添加折线

**官网中指导的代理回调方法已经弃用了，还未做出更新！**

*自定义一个绘制折线的方法，代码中的坐标需要根据实际项目需求调整*

```objc
//绘制折线
- (void) addPolyLineOnMap
{
    NSInteger count = 5;
    CLLocationCoordinate2D commonPolylineCoords[count];
    
    //起点：展滔科技大厦
    commonPolylineCoords[0].latitude = 22.639641;
    commonPolylineCoords[0].longitude = 114.038072;
    
    //北站114.02887,22.609235
    commonPolylineCoords[1].latitude = 22.609235;
    commonPolylineCoords[1].longitude = 114.02887;
    
    //民乐地铁站114.048809,22.594144
    commonPolylineCoords[2].latitude = 22.594144;
    commonPolylineCoords[2].longitude = 114.048809;
    
    //东站114.119699,22.60225
    commonPolylineCoords[3].latitude = 22.60225;
    commonPolylineCoords[3].longitude = 114.119699;
    
    //终点：坂田114.06942,22.634804
    commonPolylineCoords[4].latitude = 22.634804;
    commonPolylineCoords[4].longitude = 114.06942;
    
    //构造折线对象
    MAPolyline *commonPolyline = [MAPolyline polylineWithCoordinates:commonPolylineCoords count:count];
    
    //在地图上添加折线对象
    [self.mapView addOverlay: commonPolyline];

}
```

*实现协议中的 `mapView:rendererForOverlay:` 回调函数，设置折线样式。*

```objc
- (MAOverlayRenderer *)mapView:(MAMapView *)mapView rendererForOverlay:(id<MAOverlay>)overlay
{
    if ([overlay isKindOfClass:[MAPolyline class]]) {
        
        NSLog(@" MAPolylineRenderer 调用了");
        
        MAPolylineRenderer *  lineRenderer = [[ MAPolylineRenderer alloc] initWithOverlay:overlay];
        
        //折线宽度
        lineRenderer.lineWidth = 5.0f;
        //折线的颜色
        lineRenderer.strokeColor = [UIColor blueColor];
        //折线起始点类型
        lineRenderer.lineJoin = kCGLineJoinBevel;
        //折线终点类型
        lineRenderer.lineCap = kCGLineJoinBevel;
        
        return lineRenderer;
        
    }
    return nil;
}
```

在`ViewDidLoad`中调用该方法添加折线：

```objc
//添加折线
[self addPolyLineOnMap];
```

*编译并运行代码，会出现一条蓝色的折线。如图所示：*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/zhexian.PNG" width="320" height="568" alt="选项列表图"/>
</div>


*删除折线* 

```objc
//commonPolyline是要删除的折线，是MAPolyline的对象
[self.mapView removeOverlay:commonPolyline];
```

#### 添加圆形覆盖物

*首先，在 `viewDidLoad` 中添加如下代码来添加圆圈:*

```objc
//添加圆形覆盖物，radius指的是圆圈的半径
MACircle *Circle1 = [MACircle circleWithCenterCoordinate:CLLocationCoordinate2DMake(lati, longi) radius:500];
[self.mapView addOverlay:Circle1];
```

*然后，在协议中的 `mapView:rendererForOverlay:` 回调函数，中添加代码来设置圆圈样式*。

**将下面的代码加到 `return nil;` 的前面！**

```objc
    //如果是圆形覆盖物
    if ( [overlay isKindOfClass:[MACircle class]]) {
        
        NSLog(@" MACircleRenderer 调用了");
        
        MACircleRenderer * circleRenderer = [[MACircleRenderer alloc] initWithOverlay:overlay];
        //设置圆圈的圆边的宽度
        circleRenderer.lineWidth = 3.0f;
        //圆边的颜色
        circleRenderer.strokeColor = [UIColor colorWithRed:236/255.0 green:65/255.0 blue:110/255.0 alpha:0.8];
        //设置圆圈的填充颜色
        circleRenderer.fillColor = [UIColor colorWithRed:236/255.0 green:65/255.0 blue:110/255.0 alpha:0.3];
        
        return circleRenderer;
        
    }
```

编译运行代码，刚添加的圆圈如下所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/quanquan.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*如果想在`移动标注`的时候`删除圈圈`，然后在`新的标注点添加圆圈`，需要修改 `didChangeDragState` 代理方法中的代码！*，修改如下：

```objc
//移动标注会调用这个方法
- (void)mapView:(MAMapView *)mapView annotationView:(MAAnnotationView *)view didChangeDragState:(MAAnnotationViewDragState)newState fromOldState:(MAAnnotationViewDragState)oldState
{
    //标注移动时
    if (newState == MAAnnotationViewDragStateStarting) {
        
        NSArray * overlays = mapView.overlays;
        MACircle * cicle = [[MACircle alloc] init];
        for (NSInteger i = 0; i < overlays.count; i++) {
            
            //如果是圈圈则删除
            if ([overlays[i] isKindOfClass:[cicle class]]) {
                
                [mapView removeOverlay:overlays[i]];
            }
        }
        
    }
    //标注停止移动时
    if (newState == MAAnnotationViewDragStateEnding)
    {
        
        //获取标注新的位置所在的经纬度坐标
        CGPoint endPoint = view.centerOffset;
        
        //修正一下位置
        endPoint.x = endPoint.x + 22;
        endPoint.y = endPoint.y + 36;
        
        CLLocationCoordinate2D coo =  [mapView convertPoint:endPoint toCoordinateFromView:view];
        
        NSLog(@"移动后大头针的经纬度：latitude = %f longitude = %f",coo.latitude,coo.longitude);
        
        //添加新的圈圈
        MACircle * Circle1 = [MACircle circleWithCenterCoordinate:coo radius:500];
        [mapView addOverlay:Circle1];
        
    }
}
```

编译运行代码，移动标注后，新添加的圆圈如下所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/xinquan.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*高德地图的地图 SDK 到这里就告一段落了*

### 定位SDK的打开方式

点这里查看 [iOS 定位 SDK](http://lbs.amap.com/api/ios-location-sdk/summary/).


### 导航 SDK 的打开方式

点这里查看 [iOS 导航 SDK ](http://lbs.amap.com/api/ios-navi-sdk/summary/).

**关于导航，可以参考我的这篇博文。**

[iOS实现应用外自带地图、高德地图、百度地图导航](http://muhlenxi.com/2016/05/19/Map-Guide)

### 室内地图SDK的打开方式

点这里查看 [iOS 室内地图SDK](http://lbs.amap.com/api/ios-indoor-sdk/summary/)

### 室内定位SDK的打开方式

点这里查看 [iOS 室内定位SDK](http://lbs.amap.com/api/ios-indoorlocation-sdk/summary/)

### 结束语

*本文会阶段性的持续更新的！欢迎大家批评和指正！*

*点击下载 [本文的demo](https://github.com/muhlenXi/gaodeMapDemo)，记得给 star 支持博主的努力哦！*

*感谢阅读，有什么意见可以给我留言!*
