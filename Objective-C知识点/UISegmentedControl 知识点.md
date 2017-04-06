### UISegmentedControl 知识点 分段控制器

#### 1、创建方式

*items数组中可以有NSString或者是UIImage对象*

```objc
NSArray *array = @[@"分组", @"全部"];
UISegmentedControl *segmentedControl = [[UISegmentedControl alloc] initWithItems:array];
segmentedControl.frame = CGRectMake(100, 100, 100, 50);
```	

#### 2、设置标题 && 图片(注意下标范围从0 ~ segments-1)

```objc
[segmentedControl setTitle:@"hello" forSegmentAtIndex:1];

//设置图片
[segmentedControl setImage:[UIImage imageNamed:@"Fav_Filter_ALL_HL"] forSegmentAtIndex:0];
```	

#### 3、设置颜色

```objc
segmentedControl.tintColor = [UIColor redColor];
```

#### 4、选择分组

```objc
segmentedControl.selectedSegmentIndex = 1;
```

#### 5、插入分段

```objc
// 以标题的方式插入分段
[segmentedControl insertSegmentWithTitle:@"hao" atIndex:1 animated:YES];

// 以图片的方式插入分段
- (void)insertSegmentWithImage:(UIImage *)image atIndex:(NSUInteger)segment  animated:(BOOL)animated
```

#### 3、移除分段内容

```objc
[segmentedControl removeSegmentAtIndex:0 animated:YES]

//移除所有分段
[segmentedControl removeAllSegments];
```

#### 4、添加事件

*UIControlEventValueChanged*

```objc
[segmentedControl addTarget:self action:@selector(segmentedControlAction:) 
		forControlEvents:UIControlEventValueChanged];

```	

	