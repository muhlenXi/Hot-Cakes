### UITableView 相关知识点

#### 1、创建一个tableview 表格视图

*UITableViewStylePlain    普通的表格*

*UITableViewStyleGrouped  分组的表格*

```objc
UITableView *tableView = [[UITableView alloc] 
			 initWithFrame:self.view.bounds style:UITableViewStylePlain];
```
		 		 
#### 2、设置代理，遵循<UITableViewDelegate, UITableViewDataSource>协议

```objc
tableView.delegate = self;   
tableView.dataSource = self; 
```

#### 3、UITableViewDataSource 代理方法

*有几段？*

```objc
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;
```
*某一段有几行？*

```objc
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:	(NSInteger)section;
```

*告诉tableView在第几段的第几行要怎么显示（必须实现该方法）*

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath: (NSIndexPath *)indexPath;
```

*告诉tableView这一段的段头标题*

```objc
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection: (NSInteger)section;
```

*告诉tableView这一段的段尾标题*

```objc
- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection: (NSInteger)section;
```


*cell（indexPath）是否可以编辑*

```objc
- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
```

*tableView编辑样式时，cell编辑会触发*

> 删除模式：if (editingStyle == UITableViewCellEditingStyleDelete)
> 
> 插入模式：if (editingStyle  == UITableViewCellEditingStyleInsert)

```objc
- (void)tableView:(UITableView *)tableView commitEditingStyle:
		(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
```
	
*返回索引列表*
		
```objc
- (nullable NSArray<NSString *> *)sectionIndexTitlesForTableView:(UITableView *)tableView
```

#### 4、UITableViewDelegate 代理方法 		
*设置段头的高度*

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection: (NSInteger)section;
```

*设置段尾的高度*

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection: (NSInteger)section;
```

*告诉tableView，某一段的某一个cell要显示多高*

```objc
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath;
```

*这一行已经选中(常用)*

```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
	//取消选中这一行cell
   [tableView deselectRowAtIndexPath:indexPath animated:YES];
	//处理cell点击了的事件		
}
```

*设置段头的View*

```objc
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection: (NSInteger)section
```

*设置段尾的View*

```objc
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection: (NSInteger)section
```

*设置编辑样式*

```objc
- (UITableViewCellEditingStyle)tableView:(UITableView *)tableView 
		editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath
```


#### 5、创建一个UITableViewCell

```objc    
UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"cell"];
if (cell == nil){
	        	cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault 
				reuseIdentifier:@"cell"];
}
```

#### 6、注册cell

```objc
[tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:@"cell"];
```

*使用XIB注册cell*

```objc
[self.tableView registerNib:[UINib nibWithNibName:@"TableViewCellXIB" 	bundle:nil] forCellReuseIdentifier:@"cell"];
```

#### 6、去除表尾空白部分

```objc
tableView.tableFooterView = [[UIView alloc] init];
```

#### 7、创建表头

```objc
//自定义tableview的表头视图
self.tableView.tableHeaderView = [self createTableHeaderView];
```

#### 8、创建表尾

```objc
//使用XIB创建tableview的表尾视图
TableFooterView *tableFooterView = [[NSBundle mainBundle] 
		loadNibNamed:@"TableFooterView" owner:nil options:nil][0];
self.tableView.tableFooterView = tableFooterView;
```

#### 9、改变tableview的编辑状态

```objc
[self.tableView setEditing:!self.tableView.isEditing animated:YES];
```
#### 10、tableview滚动到对应的位置

```objc
[self.tableView scrollToRowAtIndexPath:indexPath atScrollPosition:UITableViewScrollPositionMiddle animated:YES];
```

	
#### 11、定制删除上面的文字

```objc
- (NSString *)tableView:(UITableView *)tableView 		 titleForDeleteConfirmationButtonForRowAtIndexPath:(NSIndexPath *)indexPath
```

#### 12、插入或删除某一行cell

```objc
//tableView调用
- (void)insertRowsAtIndexPaths:(NSArray *)indexPaths 
		      withRowAnimation:(UITableViewRowAnimation)animation;
		      
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths 
	              withRowAnimation:(UITableViewRowAnimation)animation;
```

#### 13、如何让指定行可以编辑

```objc
- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
```

#### 14、如何跳转到指定某一段某一行

```objc
- (void)scrollToRowAtIndexPath:(NSIndexPath *)indexPath  atScrollPosition:(UITableViewScrollPosition)scrollPosition 
			        animated:(BOOL)animated;
```

#### 15、如何移动一行	
  
```objc
- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *) sourceIndexPath toIndexPath: (NSIndexPath *)destinationIndexPath
```

#### 16、处理accessoryButton按下的事件

```objc
- (void)tableView:(UITableView *)tableView accessoryButtonTappedForRowWithIndexPath:(NSIndexPath *)indexPath
```

#### 17、设置UITableView内容下移

```objc
self.tableView.contentInset = UIEdgeInsetsMake(200, 0, 0, 0);
```
