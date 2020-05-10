### Animal

Animal.h

```objc
#import <Foundation/Foundation.h>

@interface Animal : NSObject

@property (nonatomic, copy) NSString *name;
@property (nonatomic, assign) NSInteger  age;

- (void) eat;
- (void) show;

@end
```

Animal.m

```objc
#import "Animal.h"

@interface Animal ()

@property (nonatomic, assign) double weight;

@end

@implementation Animal

- (void)eat {
    NSLog(@"Eat food ...");
}

- (void)show {
    NSLog(@"Name: %@ \n Weight: %.2fkg", self.name, self.weight);
}
```



### Dog

Dog.h

```objc
#import "Animal.h"

@interface Dog : Animal

- (void) playWithTool: (NSString *) toolName;

@end
```

Dog.m

```objc
#import "Dog.h"

@implementation Dog

- (void)eat {
    NSLog(@"Eat 🦴🦴🦴");
}

- (void)playWithTool:(NSString *)toolName {
    NSLog(@"Play With: %@", toolName);
}

@end
```



