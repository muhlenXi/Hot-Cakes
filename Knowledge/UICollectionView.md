### UICollectionView 知识点

### UICollectionViewFlowLayout 布局类

#### 1、创建一个布局

```objc
UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
```

#### 2、设置每个cell的大小

```objc
layout.itemSize = CGSizeMake(150, 150);
```

#### 3、设置最小行距和最小列距

```objc
layout.minimumLineSpacing = 20;       //最小行距,默认为10
layout.minimumInteritemSpacing = 100; //最小的列距，默认为10
```

#### 4、设置滚动方向

*UICollectionViewScrollDirectionVertical = 垂直方向*

*UICollectionViewScrollDirectionHorizontal = 水平方向*
    	
```objc
layout.scrollDirection = UICollectionViewScrollDirectionHorizontal;
```

### UICollectionView

#### 5、创建 UICollectionView

```
UICollectionView *collectionView = [[UICollectionView alloc]  initWithFrame:self.view.bounds collectionViewLayout:layout];
```

#### 6、设置代理 

*需要遵循<UICollectionViewDataSource, UICollectionViewDelegate,UICollectionViewDelegateFlowLayout>协议*

```objc
collectionView.dataSource = self;	    //数据源代理
collectionView.delegate = self;		//事件代理
```

#### 7、注册cell

```objc
[collectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:@"cell"];
```

*使用XIB注册cell*

```objc
[self.collectionView registerNib:[UINib nibWithNibName:@"CollectionViewCellXIB" bundle:nil] forCellWithReuseIdentifier:@"cellXIB"];
```

#### 8、取出cell

```objc
UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"cell" forIndexPath:indexPath];
```

#### 9、设置cell的背景色

```objc
cell.contentView.backgroundColor = [UIColor redColor];
```

#### 10、注册段头   UICollectionElementKindSectionHeader

```objc
[collectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"header"];
```

*使用XIB注册段头*

```objc
[self.collectionView registerNib:[UINib nibWithNibName:@"HeaderView"  bundle:nil] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader  withReuseIdentifier:@"header"];
```

#### 11、注册段尾   UICollectionElementKindSectionFooter

```objc
[collectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"footer"];
```
	
*使用XIB注册段尾*

```objc
[self.collectionView registerNib:[UINib nibWithNibName:@"FooterView" bundle:nil] forSupplementaryViewOfKind:UICollectionElementKindSectionFooter withReuseIdentifier:@"footer"];
```

#### 12、给Cell添加边框

```objc
self.contentView.layer.borderWidth = 1;
self.contentView.layer.borderColor = [UIColor colorWithRed:173.0f/255.0f green:174.0f/255.0f blue:175.0f/255.0f alpha:1.0f].CGColor;
```

#### 13、纯代码自定义CollectionViewCell

```objc
//tableView的复用队列创建cell调用initWithFrame
- (instancetype)initWithFrame:(CGRect)frame
{
    if (self = [super initWithFrame:frame]) {
        
        //添加cell需要的控件
        
        self.imageView = [[UIImageView alloc] initWithFrame:CGRectMake(1, 1, frame.size.width-2, frame.size.height-2)];
        [self.contentView addSubview:self.imageView];
        
        self.imageView.contentMode = UIViewContentModeScaleAspectFill;
        self.imageView.clipsToBounds = YES;
        
        self.contentView.layer.borderWidth = 1;
        self.contentView.layer.borderColor = [UIColor colorWithRed:173.0f/255.0f green:174.0f/255.0f blue:175.0f/255.0f alpha:1.0f].CGColor;   
        
    }
    
    return self;
}
```


#### 14、一般需要实现的代理方法

```objc
#pragma mark -  UICollectionViewDataSource

// collectionView中Item的个数
- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
{
    return self.collectionDataArray.count;
}

// collectionView中如何显示每个cell
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
{
	 //这是自定义的cell
    CollectionViewCell * cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"CollectionViewCell" forIndexPath:indexPath];
    
    //这里配置cell
    
    return cell;
}

#pragma mark -  UICollectionViewDelegateFlowLayout

// 每个cell的大小
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath
{
    return CGSizeMake(150, 150);
}

// cell的行距
- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumLineSpacingForSectionAtIndex:(NSInteger)section
{
    return 20;
}

// cell的列距
- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumInteritemSpacingForSectionAtIndex:(NSInteger)section
{
    return 20;
}

// cell显示的区域整体缩进的范围
- (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section
{
    return UIEdgeInsetsMake(20, 20, 20, 20);
}

#pragma mark -  UICollectionViewDelegate

// 每个cell的点击响应
- (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath
{
  
    //这里处理cell的点击事件
        
}
```
	