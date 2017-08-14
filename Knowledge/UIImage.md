### UIImage 知识点

#### 1、改变图片的大小        

```objc
//源图片
UIImage * image = [UIImage imageNamed:@"0"];
//更改后的尺寸
CGSize size = CGSizeMake(120, 120);
UIGraphicsBeginImageContext(size);
[image drawInRect:CGRectMake(0, 0, size.width, size.height)];
//更改后的图片
UIImage * newImage = UIGraphicsGetImageFromCurrentImageContext();
UIGraphicsEndImageContext();
```
#### 2、图片拉伸不变形

```objc
UIImage * image = [UIImage imageNamed:@"图片名"];
UIEdgeInsets  insets = UIEdgeInsetsMake(10, 10, 10, 10);
[image resizableImageWithCapInsets:insets resizingMode:UIImageResizingModeStretch];
```
#### 3、避免图片渲染成蓝色

```objc
UIImage * selectedImage = [[UIImage imageNamed:@"name"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
```
