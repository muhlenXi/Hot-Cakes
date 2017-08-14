#### 命名规范

【0】属性的参数应该按照下面的顺序排列：原子性，读写和内存管理。

```objc
@property (nonatomic, readwrite, copy) NSString *name;
```

【1】如果方法表示让对象执行一个动作，使用动词打头来命名。如用 logIn 表示登陆。

【2】方法中不要用 with 来连接两个参数。

```objc
- (instancetype)initWithName:(NSString *)name age:(NSUInteger)age;
```

*通常是使用 withA:andB: 这种命名，用来表示方法执行了两个相对独立的操作,但是尽量不要使用，应遵守单一原则，拆分为两个独立的方法。*

```objc
- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;
```