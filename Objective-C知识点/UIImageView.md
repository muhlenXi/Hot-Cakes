### UIImageView相关知识点

#### 1、UIImageView的Image填充样式

*UIViewContentModeScaleToFill        拉伸内容,会导致内容变形*

*UIViewContentModeScaleAspectFit     拉伸内容,内容比例不变*

*UIViewContentModeScaleAspectFill    拉伸内容,内容比例不变,但是有可能部分内容不能显示*

```objc
self.headerView.contentMode = UIViewContentModeScaleAspectFill;
```

#### 2、初始化UIImageView并设置大小

```objc
UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(100, 	
		100, 200, 150)];
```

#### 3、获取图片路径

bundle管理资源的对象，也是单例  参数1:资源名字，参数2:资源类型
      	   
```objc
NSString *path = [[NSBundle mainBundle] pathForResource:@"bg" ofType:@"jpg"]; //方式1
NSString *path = [[NSBundle mainBundle] pathForResource:@"bg.jpg" ofType:nil] //方式2
```

#### 4、根据路径获取 图片

```objc
//创建图片对象，不会缓存， 适用于大图片，不常使用的图片
UIImage *image = [[UIImage alloc] initWithContentsOfFile:path];

//创建图片对象，会对图片缓存，适用于小图片，常用的图片
UIImage *image1 = [UIImage imageNamed:@"bg.jpg"];
```
#### 5、设置内容填充模式

```objc
//图片不会拉伸，将图片铺满整个imageVIew
imageView1.contentMode = UIViewContentModeScaleAspectFill;
```		

#### 6、打开高亮状态 && 并设置高亮图片

```objc
imageView1.highlighted = YES;
mageView1.highlightedImage = [UIImage imageNamed:@"detail_highlight@2x"];
```		
