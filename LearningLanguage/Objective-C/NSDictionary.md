### NSDictionary

```objc
 NSDictionary *dict = @{@(1) : @"one", @(2) : @"two", @(3) : @"threee", @(4) : @"four"};
```

### NSMutableDictionary

```objc
NSMutableDictionary *textAttrs = [NSMutableDictionary dictionary];

NSMutableDictionary *textAttrs = [NSMutableDictionary dictionaryWithDictionary:dict];

NSMutableDictionary *localDict = [dict mutableCopy];
```

