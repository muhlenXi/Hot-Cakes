---
title: iOS APP 多语言支持
date: 2016-07-07 16:47:19
categories: blog
tags: [多语言,Objective-C]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/07/07/Localized-Language](http://muhlenxi.com/2016/07/07/Localized-Language)*

### 导语：

> APP 根据手机系统的语言进行显示。第二种是在 APP 内自行设置语言，也就是应用内切换语言，设置的是什么语言，就显示什么语言。

  写个 Demo 玩一下，实践是检验真理的唯一标准。

<!-- more -->

### 基础文件配置:　　

####  1、首先是添加 APP 的本地化语言支持。

方法：`PROJECT` -> `Info` -> `Localizations` -> 点击 `+` 添加。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/addLocalization.png" width="719" height="473" alt="选项列表图"/>
</div>

####  2、添加 APP 名称本地化文件

**文件的名字一定要是 `InfoPlist`！**

方法：`File` -> `New` -> `File...` -> `iOS`-> `Resource` -> `Strings File`

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/StringFiles.png" width="675" height="276" alt="选项列表图"/>
</div>

####  3、设置 InfoPlist.strings 的 Localization

方法：在项目中找到并选中 `InfoPlist.strings` 文件，在靠右边的窗口中，选择支持的语言类型。

*本例中选择的是英语、简体中文、繁体中文*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/localizationType.png" width="260" height="93" alt="选项列表图"/>
</div>

####  4、设置对应文件的 key-value 键值对

点击 `InfoPlist.strings` 左边的三角形符号展开该文件，分别在对应的 strings 文件中设置对应的键值对。

*4-1、在 InfoPlist.strings(English) 文件中输入：*

```objc
"CFBundleDisplayName" = "EnglishName";
```

*4-2、在 InfoPlist.strings(Chinese(Simplified)) 文件中输入：*

```objc
"CFBundleDisplayName" = "CFBundleDisplayName" = "中文名字";
```

*保存后编译并运行就可以了。*

###  系统语言切换:

####  创建 Localizable 字符串文件

同样的步骤，执行2-3步骤，创建一个名字为 `Localizable` 的strings文件，然后设置 Localizable.strings 的 Localization 。

然后执行步骤4，设置不同语言中项目所需的字符串。

*本例中，我们需要设置 Alarm clock、Medicine、Setings、Find Band、test words 等字符串。*

也就是说：

*1-在 Localizable.strings(English) 文件中输入：*

```objc
"Alarm clock" = "Alarm clock";
"Medicine" = "Medicine";
"Setings"  = "Setings";
"Find Band" = "Find Band";

"test words" = "test words";
```

*2-在 Localizable.strings(Chinese(Simplified)) 文件中输入：*

```objc
"Alarm clock" = "闹钟提醒";
"Medicine" = "吃药提醒";
"Setings"  = "设置";
"Find Band" = "寻找手环";

"test words" = "测试语句";
```

####  调用 `NSLocalizedString(key, comment)`方法

举例：

```objc
self.descriptionLabel.text = NSLocalizedString(@"test words", @"描述文字");
```

*到这里第二部分的内容基本就完了。*

###  应用内切换语言:

> 应用内切换语言的思路是：每当我们添加一种语言的支持后，系统就会在我们的项目中生成对应语言的`.lproj` 支持文件。我们将 `NSUserDefaults` 将用户的语言设置保存到本地中，保存的 String 的值和下图中的文件名一样。然后通过 `NSBundle` 根据 `.lproj` 支持文件生成 bundle，最后通过 `NSLocalizedStringFromTableInBundle` 方法获取对应的 String 的值。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/lproj.png" width="444" height="113" alt="选项列表图"/>
</div>

####  具体的做法如下：

*为了方便使用，我们新建一个继承 `NSObject` 的 `XYJLanguageTool` 类，用来管理我们的语言。*

* `XYJLanguageTool.h` 中的代码如下：*

```objc
#import <Foundation/Foundation.h>

#define XYJGetStringWithKey(key) [[XYJLanguageTool sharedInstance] getStringForKey:key]

@interface XYJLanguageTool : NSObject

@property (nonatomic,strong,readonly) NSBundle * bundle;

// 单例初始化方法
+ (id) sharedInstance;

// 根据key获取相应的String
- (NSString *) getStringForKey:(NSString *) key;

// 应用内设置新语言
- (void) setNewLanguage:(NSString *) language;

@end
```

* `XYJLanguageTool.m` ,中的代码如下：*

```objc

#import "XYJLanguageTool.h"

#define Language_Key @"languageKey"
#define Chinese_Simple @"zh-Hans"
#define Chinese_Traditional @"zh-Hant"
#define English_US @"en"

@implementation XYJLanguageTool

+ (id)sharedInstance
{
    static dispatch_once_t onceToken;
    static XYJLanguageTool * languageTool;
    dispatch_once(&onceToken, ^{
        
        languageTool = [[XYJLanguageTool alloc] init];
    
        
    });
    return languageTool;
}

// 根据语言名获取bundle
- (NSBundle *)bundle
{
    NSString * setLanguage = [[NSUserDefaults standardUserDefaults] objectForKey:Language_Key];
    //默认是简体中文
    if (setLanguage == nil) {
        setLanguage = Chinese_Simple;
    }
    NSString * bundlePath = [[NSBundle mainBundle] pathForResource:setLanguage ofType:@"lproj"];
    
    return [NSBundle bundleWithPath:bundlePath];
}

// 根据key获取value
- (NSString *)getStringForKey:(NSString *)key
{
    NSBundle * bundle = [[XYJLanguageTool sharedInstance] bundle];
    if (bundle) {
        
        return NSLocalizedStringFromTableInBundle(key, @"Localizable", bundle, @"HelloWord");
    }
    return NSLocalizedString(key, @"HelloWord");
}

- (void)setNewLanguage:(NSString *)language
{
    NSString * setLanguage = [[NSUserDefaults standardUserDefaults] objectForKey:Language_Key];
    if ([language isEqualToString:setLanguage]) {
        return;
    }
    // 简体中文
    else if ([language isEqualToString:Chinese_Simple]) {
        [[NSUserDefaults standardUserDefaults] setObject:Chinese_Simple forKey:Language_Key];
        [[NSUserDefaults standardUserDefaults] synchronize];
    }
    // 繁体中文
    else if ([language isEqualToString:Chinese_Traditional]) {
        [[NSUserDefaults standardUserDefaults] setObject:Chinese_Traditional forKey:Language_Key];
        [[NSUserDefaults standardUserDefaults] synchronize];
    }
    // 英文
    else if ([language isEqualToString:English_US]) {
        [[NSUserDefaults standardUserDefaults] setObject:English_US forKey:Language_Key];
        [[NSUserDefaults standardUserDefaults] synchronize];
    }
    
    // 发送更新语言的通知，用于重新设置Window的RootViewController
    [[NSNotificationCenter defaultCenter] postNotificationName:@"UpDateLanguageUI" object:nil];
}

```
####  使用方法如下（记得导入头文件）：

*通过调用 `XYJGetStringWithKey` 方法来获取相应的字符串。*

举例如下：

```objc
self.navigationItem.title = XYJGetStringWithKey(@"Alarm clock");
```

###  最后的真机运行图如下:

*应用内切换为英语*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/english.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*应用内切换为简体中文*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/jianti.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*应用内切换为繁体中文*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/fanti.PNG" width="320" height="568" alt="选项列表图"/>
</div>


*本例中的demo已经提交到Github上，点击[这里下载](https://github.com/YinjunXi/multipleLanguageDemo)，欢迎提出批评和指正，最后谢谢大家！*