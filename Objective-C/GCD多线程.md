### GCD多线程

#### 全局队列中执行

```objc
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
    // Do something
});
```

#### 主线程中执行

```objc
dispatch_async(dispatch_get_main_queue(), ^{
        
    // Do something
});
```

#### 一次线程中执行

```objc
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
        
	//code to be executed once
});
```

#### 延迟n秒执行

```objc
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(n * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        
	//code to be executed after a specified delay
});
```

#### 自定义线程中执行

```objc
dispatch_queue_t myQueue = dispatch_queue_create("muhlenxi.com", NULL);
dispatch_async(myQueue, ^{
        
	// Do something
});
```

#### 多线程并行执行

```objc
dispatch_group_t group = dispatch_group_create();
dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
    // Do something 1
});

dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
    // Do something 2
});

dispatch_async(dispatch_get_global_queue(0, 0), ^{
        
    // Do something 3
});

dispatch_group_notify(group, dispatch_get_global_queue(0, 0), ^{
        
    // Comprehensive processing result
});
```
