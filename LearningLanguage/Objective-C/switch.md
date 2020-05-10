### switch

```objc
  Sex sex = SexUnknow;
  switch (sex) {
      case SexMale: {
          NSLog(@"A man")
      }
          break;
      case SexFemal: {
          NSLog(@"A women")
      }
      default:
          NSLog(@"Unknown")
          break;
  }
```

Sex

```objc
typedef NS_ENUM(NSUInteger, Sex) {
    SexMale,
    SexFemal,
    SexUnknow,
};
```

