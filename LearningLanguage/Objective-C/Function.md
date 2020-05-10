### 0 - Init

```objc
- (instancetype)init {
    self = [super init];
    if (self) {
        // Init your instance
    }
    return self;
}

+ (instancetype)sharedKit {
    static NIMKit *instance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        instance = [[NIMKit alloc] init];
    });
    return instance;
}
```

### 1 - Static

```objc
+ (NSString *)getAppTempPath {
    return NSTemporaryDirectory();
}
```

### 2 - Normal

```objc
// 无参
- (void)setupSubviews{ 
	// Do something
}

// 1 参
- (void)viewWillAppear:(BOOL)animated {
  // Do something
}

// 2 参
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event {
  // Do something
}

// 无参有返回值
- (NSString *)contentText {
    return self.toolBar.contentText;
}
```

### 3 - Lazy

```objc
- (UIButton *)deleteBtn {
    if (_deleteBtn == nil) {
        _deleteBtn = [UIButton buttonWithType:UIButtonTypeCustom];
        [_deleteBtn setImage:[UIImage imageNamedInModuleChat:@"chat_dellte"] forState:UIControlStateNormal];
    }
    return _deleteBtn;
}
```





 



