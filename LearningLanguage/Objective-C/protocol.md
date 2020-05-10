### Define Protocol

```objc
@protocol NIMInputDelegate <NSObject>
- (void)showInputView;
- (void)hideInputView;
  
@optional
- (void)inputViewSizeToHeight:(CGFloat)toHeight showInputView:(BOOL)show;
@end
```



### 遵循协议

新建一个 InputView 文件如下：

InputView.h

```objc
#import <UIKit/UIKit.h>

@interface InputView : UIView

@end
```

InputView.m

```objc
#import "InputView.h"
#import "NIMInputView.h"

@interface InputView () <NIMInputDelegate>

@end

@implementation InputView


#pragma mark - NIMInputDelegate

- (void) showInputView {
    // showInputView
}

- (void) hideInputView {
    // hideInputView
}

// 可选方法，可以不实现
- (void)inputViewSizeToHeight:(CGFloat)toHeight showInputView:(BOOL)show {
    
}

@end
```



