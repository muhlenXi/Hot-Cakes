### 	UIScrollView 知识点

#### 1、创建可滚动视图

```objc
UIScrollView * scrollView = [[UIScrollView alloc] initWithFrame:self.view.bounds];
scrollView.backgroundColor = [UIColor greenColor];
```

#### 2、将需要展示内容放到ScrollView上

```objc
UIImageView * imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, 1000, 750)];
imageView.image = [UIImage imageNamed:@"img.jpg"];
[scrollView addSubview:imageView];
```

#### 3、设置可滚动的范围

*只有contentSize > scrollView的frame时，才可以滚动*

```objc
scrollView.contentSize = CGSizeMake(1000, 750);
```

#### 4、设置偏移量

```objc
scrollView.contentOffset = CGPointMake(100, 100);
```

#### 5、设置弹簧效果

```objc
scrollView.bounces = NO;

//不可以滚动时也有弹簧效果（前提scrollView.bounces为YES）
scrollView.alwaysBounceHorizontal = YES;  //水平方向
scrollView.alwaysBounceVertical = YES;    //垂直方向
```		

#### 6、设置显示进度条

```objc
scrollView.showsHorizontalScrollIndicator = YES; //水平方向进度条
scrollView.showsVerticalScrollIndicator = YES;   //垂直方向进度条
```

#### 7、设置进度条样式

```objc
scrollView.indicatorStyle = UIScrollViewIndicatorStyleBlack;
```

#### 8、设置分页效果

```objc
scrollView.pagingEnabled = YES;
```

#### 9、设置是否可以滚动

```objc
scrollView.scrollEnabled = YES;
```

#### 10、设置是否可以滚动到顶部

```objc
scrollView.scrollsToTop = YES;
```

#### 11、滚动到指定位置

```objc
scrollView.contentOffset = CGPointMake(200, 600);

[scrollView setContentOffset:CGPointMake(100, 100) animated:YES];
```

#### 12、滚动到指定区域 可视

```objc
[scrollView scrollRectToVisible:CGRectMake(400, 400, 100, 100) animated:YES];
```

#### 13、设置缩放的倍率 

```objc
scrollView.minimumZoomScale = 0.5;   //最小
scrollView.maximumZoomScale = 2;     //最大
```

#### 14、创建分页控件

```objc
UIPageControl *pageControl = [[UIPageControl alloc] initWithFrame:CGRectMake(100, 300, 200, 50)];
// 设置页数
pageControl.numberOfPages = 4;
// 设置当前页数
pageControl.currentPage = 2;

// 设置pageControl的颜色
pageControl.currentPageIndicatorTintColor = [UIColor redColor];		//选中的颜色
pageControl.pageIndicatorTintColor = [UIColor greenColor]; 			//其它点的颜色
```

#### 15、UIScorllView的常用代理方法

* 即将开始拖拽

```objc
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
```
* 已经停止减速
 
```objc
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView
```
* 点击状态栏回到顶部
 
```objc
- (BOOL)scrollViewShouldScrollToTop:(UIScrollView *)scrollView
```
* 已经滑到顶部
 
```objc
- (void)scrollViewDidScrollToTop:(UIScrollView *)scrollView
```
* 放大缩小
 
```objc
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView
```

* 缩放完毕

```objc
- (void)scrollViewDidEndZooming:(UIScrollView *)scrollView withView:(UIView *)view atScale:(CGFloat)scale
```
