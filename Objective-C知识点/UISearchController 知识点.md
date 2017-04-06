### UISearchController 知识点

#### 1、创建一个搜索控制器

*SearchResultsController : 显示结果的视图控制器，如果为nil，是使用的当前的视图控制器。*

```objc
self.searchController = [[UISearchController alloc] initWithSearchResultsController:nil];
```

#### 2、使用一个视图控制器初始化UISearchController

```objc
ReslutTableViewController *resultVC = [[ReslutTableViewController alloc] init];
UISearchController *searchController = [[UISearchController alloc] initWithSearchResultsController:resultVC];
```

#### 3、设置搜索框的背景色

```objc
self.searchController.searchBar.barTintColor = [UIColor purpleColor];
```

#### 4、设置代理

*需要遵循的协议 `UISearchResultsUpdating`和`UISearchControllerDelegate`*

```objc
self.searchController.delegate = self;
self.searchController.searchResultsUpdater = self;
```

#### 5、设置提示文字

```objc
searchController.searchBar.placeholder = @"请输入关键字";
```
