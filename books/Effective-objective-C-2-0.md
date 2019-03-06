---
title: Effective objective-C 2.0 学习笔记
date: 2015-11-10 14:52:41
categories: 学习笔记
tags: [编写高质量iOS与OS X代码的52个有效方法,学习笔记]
---


 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2015/11/10/Effective-Objective-C-2-0](http://muhlenxi.com/2015/11/10/Effective-Objective-C-2-0)*

#### 前言：

> Effective objective-C,翻译过来是编写高质量 iOS 与 OS X 代码的52个有效方法，作者是 Matt Galloway ，本书是世界级 C++ 开发大师 Scott Meyers 亲自担当顾问编辑的 “Effective Software Development Series” 系列丛书中的新作，Amazon 全五星评价。全书从语法、接口与API设计、内存管理、框架等7大方面总结和探讨了 Objective-C 编程中 52 个鲜为人知的和容易被忽视的特性和陷阱！

学习贵在记录和总结收获！点击阅读全文了解更多！　　

<!-- more -->

Objective-C 源自 Smalltalk ，是一门相当动态的语言，代码是在运行期（runtime）执行的！


#### 第1条：了解Objective-C语言的起源

*C++,Java等使用的是function calling ，运行所执行的代码由编译器来决定。如果调用的函数是多态的，那么在运行时就要按照 virtual table (虚方法表) 来查出应该执行哪个函数实现。*

*Objective-C语言使用的是动态绑定的 message structure (消息结构) ，其运行时所执行的代码由运行环境来决定；无论多态，总是在运行时才会去检查对象的类型和查找所要执行的方法。*

Objective-C 语言中的对象是用来指示对象的。

举个🌰 ：

```objc
NSString * str = @"hello world";
```

*对象所占的内存是分配在堆空间中！*

*指向对象的指针变量分配在栈上！*

*一般不含星号的变量也分配在栈上!* 如 CGRect，CGSize...

#### 第2条：在类的头文件中尽量少引用其他头文件

> **除非确有必要，否则不要引入头文件！使用前向声明！**

解决办法：在类的头文件中使用`前向声明`提到的类。并在.m文件中引入那些类的头文件。

好处：降低了类之间的耦合度，减少了编译时间，

```objc
@class  XYJMyMenuTVC;    //前向声明语法
```
*有时无法使用前向声明，比如要声明某个类遵循一项协议。此时，尽量把`该类遵循某协议`的声明已到`匿名类`中。如果不行的话，就把协议单独放在一个头文件中，然后将其引入。*

#### 第3条：多用字面量语法，少用与之等价的方法

> * *应该使用字面量语法来创建字符串、数值、数组、字典。与创建此类对象的常规方法相比，这么做更加简明扼要。*
* *应该通过取下标操作来访问数组下标或字典中键所对应的元素。*
* *用字面量语法创建数组或字典，若值中有 nil，则会抛出异常。因此，务必确保值里不含 nil ！*

*字面量语法举例：*

举个🌰 ：

```objc
 NSString * string = @"hello world";
    
 NSNumber * intNumber = @1;
 NSNumber * floatNumber = @2.5f;
 NSNumber * doubleNumber = @3.14159;
 NSNumber * boolNumber = @YES;
 NSNumber * charNUmber = @'A';
    
 NSArray * array = @[@"1",@"2"];
 NSDictionary * dic = @{@"keyone":@"1",@"keytwo":@"2"};
```

*不可变对象转为可变对象：*

举个🌰 ：

```objc
NSMutableArray * arrayM = [@[@"1",@"2"] mutableCopy];
NSMutableString * strM = [@"hello" mutableCopy];
NSMutableDictionary * dicM = [@{@"one":@"1"} mutableCopy];
```

#### 第4条：多用类型常量，少用#define预处理指令

> * *不要用预处理指令定义常量。这样定义出来的常量不含类型信息，编译器只是会在编译前据此查找与替换操作。即使有人重新定义了常量值，编译器也不会产生警告信息，这将导致应用程序中的常量值不一致。*
* *在实现文件中使用 `static const`  来定义 `只在编译单元内可见的常量`。由于此类常量不在全局符号表中，所以无须为其名称加前缀。*
* *在头文件中使用 `extern` 来声明全局常量，并在相关实现文件中定义其值。这种常量会出现在全局符号表中，所以其名称应加以区隔，通常用与之相关的类名称做前缀。*


*常量名称的常用命名法是：若常量局限于某编译单元内，也就是实现文件之内，则在前面加字母 k ；*

*若不打算公开某个常量，则应将其定义在使用该常量的实现文件里*

*变量一定要同时用 static 与 const 来声明，如果试图修改由 const 修饰符所声明的变量，那么编译器就会报错！*

举个🌰 ：

```objc
static const NSTimeInterval kAnimationDuration = 0.3;  //定义一个类型为NSTimeInterval的时间常量
```
*若常量在类之外可见，则通常以类名为前缀。*

*常量定义从右往左解读，注意 const 修饰符在常量类型中的位置。*

举个🌰 ：

```objc
extern NSString * const XYJLoginManagerDidLoginNotification;  //在.h文件中声明

NSString * const XYJLoginManagerDidLoginNotification = @"XYJLoginManagerDidLoginNotification";  //在.m文件中定义
```

#### 第5条：用枚举表示状态、选项、状态码

>* *应该用枚举来表示状态机的状态、传递给方法的选项以及状态码等值，给这些值起个易懂的名字吧。*
* *如果把传递给某个方法的选项表示为枚举类型，而多个选项又可同时使用，那么就将各选项值定义为 2 的幂，以便通过按位或操作将其组合起来。*
* *用 NS_ENUM与NS_OPTIONS 宏来定义枚举类型，并指明底层数据类型。这样做可以确保枚举是用开发者所选的底层数据类型实现出来的，而不会采用编译器所选的类型。*
* *在处理枚举类型的 switch 语句中不要实现 default 分支，这样的话，加入新枚举之后，编译器就会提示开发者：switch 语句并未处理所有枚举。*

**凡是需要以 按位或 操作来组合的枚举都应使用 NS_OPTIONS 定义，若是枚举不需要互相组合，则应使用 NS_ENUM 来定义**

举个🌰 ：

```objc
typedef enum : NSUInteger {
    ConnectionStateDisconnected ,
    ConnectionStateConnecting,
    ConnectionStateConnected,
} TCPConnectionState;

typedef NS_ENUM (NSUInteger,UDPConnectionState) {
    UDPConnectionStateDisconnected,
    UDPConnectionStateConnecting,
    UDPConnectionStateConnected,
};

typedef NS_OPTIONS(NSUInteger, PermittedDirection){
    PermittedDirectionUp    = 1 << 0,
    PermittedDirectionDown  = 1 << 1,
    PermittedDirectionLeft  = 1 << 2,
    PermittedDirectionRight = 1 << 3,
    
};
```

#### 第6条：理解属性这一概念

> * *可以用@property语法来定义对象中所封装的数据。*
* *通过 “attribute（特质)” 来指定存储数据所需的正确语义*。
* *在设置属性所对应的实例变量时，一定要遵从该属性所声明的语义*。
* *开发 iOS 程序时应该使用 noatomic 属性，因为 atomic 属性会严重影响性能*。

*在对象之间传递数据并执行任务的过程就叫做"消息传递"（Messaging）。*

*当应用程序运行起来，为其提供相关支持的代码叫做 “Objective-C runtime”（运行期环境）*

*在类的的实现代码里可以通过 `@synthesize` 语法来指定实例变量的名字*

举个🌰 ：  *要写在@implementation的下面*

```objc
@synthesize firstName = _myFirstName;  //将生成的实例变量命名为_myFirstName
```

*在类的的实现代码里可以通过 `@dynamic` 语法来阻止编译器自动合成存取方法*

举个🌰 ：  *要写在 @implementation 的下面*

```objc
@dynamic firstName;  
```

属性的特质：

* 1.原子性（atomic / noatomic）
* 2、读写权限 （readwrite / readonly）
* 3、内存管理语义（assign / strong / weak / unsafe_unretained / copy）
* 4、方法名 （setter = 名字 / getter = 名字）


#### 第7条：在对象内部尽量直接访问实例变量

> * *在对象内部 `读取数据` 时，应该直接通过 `实例变量` 来读，而在 `写入数据`时，则应通过 `属性` 来写。*
* *在 `初始化方法` 和 `dealloc` 方法中，总是应该直接通过 `实例变量` 来读写数据。*
* *有时候会使用 `懒加载` 配置某个数据，这种情况下，需要通过 `属性` 来读取数据。*


#### 第8条：理解 "对象等同性" 这一概念

> * *若想检测的对象的等同性，请提供 `isEqual:` 和 `hash` 方法。*
* *相同的对象必须具有相同的哈希码，但是两个哈希码相同的对象却未必相同。*
* *不要盲目地逐个检测每条属性，而是依照具体需求来制定检测方案。*
* *编写 hash 方法时，应该使用计算速度快而且哈希码碰撞几率低的算法。*

#### 第9条：以“类族模式” 隐藏实现细节

> * *类族模式可以把实现细节隐藏在一套简单的公共接口后面。*
* *系统框架中经常使用类族。*
* *从类族的公共抽象基类中继承子类时要当心，若有开发文档，则应首先阅读。*

*需要向类族中新增实体子类，需要遵守的几条规则：*

* 子类应该继承自类族的抽象基类。
* 子类应该定义自己的数据存储方式。
* 子类应当覆写超类文档中指明需要覆写的方法。

#### 第10条：在既有类中使用关联对象存放自定义数据

> * *可以通过“关联对象”机制来把两个对象关联起来。*
* *定义关联对象时可指定内存管理语义，用以模仿定义属性时所采用的“拥有关系” 和 “非拥有关系”。*
* *只有其他做法不可行时才应选用关联对象，因为这种做法通常会引入难以查找的bug。*

**对象关联类型：**

| 关联对象  | 等效的@property属性           | 
| ------------- |:-------------:|
| OBJC\_ASSOCIATION_ASSIGN                 | assign | 
| OBJC\_ASSOCIATION\_RETAIN_NONATOMIC      | nonatomic,retain|  
| OBJC\_ASSOCIATION\_COPY_NONATOMIC        | nonatomic,copy|
| OBJC\_ASSOCIATION_RETAIN                 | retain | 
| OBJC\_ASSOCIATION_COPY                   | copy |    


**管理关联对象的方法：**

*以给定的键和策略为某对象设置关联对象值*

```objc
objc_setAssociatedObject(id object, void *key, d value, objc_AssociationPolicy policy)
```

*从某对象中根据键获取设置关联对象的值*

```objc
objc_getAssociatedObject(id object, const void *key)
```

*移除指定对象的全部关联对象*

```objc
objc_removeAssociatedObjects(id object)
```

#### 第11条：理解objc_msgSend的作用

> * *消息由接收者、选择子及参数构成。给某对象发送消息（invoke a message）也就相当于在该对象上调用方法 (call a method)。*
* *发给某对象的全部消息都要由"动态消息派发系统（dynamic message dispatch system）来处理，该系统会查出对应的方法，并执行其代码。"*

`objc_msgSend` 函数会依据接收者与选择子的类型来调用适当的方法。为了完成此操作，该方法需要在接收者所属的类中搜寻其"方法列表"（list of methods），如果能找到与选择子名称相符的方法，就跳至其实现代码。如果找不到，那就沿着继承体系继续向上查找，等找到合适的方法之后再跳转。如果做种还是找不到相符的方法，那就执行“消息转发” (message forwarding) 操作。

#### 第12条：理解消息转发机制

> * 若对象无法响应某个选择子，则进入消息转发流程。
* 通过运行期的动态方法解析功能，我们可以在需要用到某个方法时再将其加入类中。
* 对象可以把其无法解读的某些选择子转交给其他对象来处理。
* 经过上述两步之后，如果还是没办法处理选择子，那就启动完整的消息转发机制。

消息转发分为两大阶段：

第一阶段：*先征询接收者所属的类，看其是否能动态添加方法，以处理这个未知的选择子 (unknow selector)，这叫做动态方法解析（dynamic selector resolution）。*

第二阶段：(完整的消息转发机制 full forwarding mechanism )

如果运行期系统已经把第一阶段执行完了，那么接收者自己就无法再以动态新增方法的手段来相应包含该选择子的消息了，这时，运行期系统会请求接收者以其他手段来处理与消息相关的方法调用。

第一步：*如果有其他对象能够处理这条消息，则把消息转给那个对象，消息转发过程结束，一切如常。*

第二步：*如果没有（replacement receiver），则启动完整的消息转发机制，运行期系统会把与消息有关的全部细节都封装到 NSInvocation 对象中，再给接收者最后一次机会，令其设法解决当前还未处理的这条消息。*

*对象收到无法解读的消息后，首先会调用其所属类的类方法：*

```objc
+(BOOL)resolveInstanceMethod:(SEL)sel
```

#### 第13条：用方法调配技术(method swizzling)调试黑盒方法

> * 在运行期，可以向类中新增或替换选择子所对应的方法实现。
* 使用另一份实现来替换原有的方法实现，这道工序叫做"方法调配"，开发者常用此技术向原有实现中添加新功能。
* 一般来说，只有调试程序的时候才需要在运行期修改方法实现，这种做法不宜滥用。


*交换方法实现，举个 🌰：*

```objc
//获取lowercaseString的方法实现
Method originalMethod = class_getInstanceMethod([NSString class], @selector(lowercaseString));

//获取自定义的xyj_myLowercaseString的方法实现
Method swappedMethod = class_getInstanceMethod([NSString class], @selector(xyj_myLowercaseString));

//交换两个方法的实现
method_exchangeImplementations(originalMethod, swappedMethod);
```

#### 第14条：理解"类对象"的用意

> * 每个实例都有一个指向 Class 对象的指针，用以表明其类型，而这些 Class 对象则构成了类的继承体系。
* 如果对象的类型无法在编译期确定，那么就应该使用类型信息查询方法来探知。
* 尽量使用类型查询方法来确定对象类型，而不要直接比较类对象，因为某些对象可能实现了消息转发功能。

*每个对象结构体的首个成员是Class类的变量，该变量定义了对象所属的类，通常称之为`is a`指针。* 

*`isMemberOfClass:`判断对象是否为某个特定类的实例。*

*`isKindOfClass:`判断对象是否为某类或其派生类的实例。*

#### 第15条：用前缀避免命名空间冲突

> * 选择与你的公司、应用程序或二者皆有关联之名称作为类名的前缀，并在所有代码中均使用这一前缀。
> * 若自己所开发的程序库中用到了第三方库，则应为其中的名称加上前缀。

*Apple宣称其保留使用所有“两字母前缀（two-letter prefix）”的权利，所以你自己选用的前缀应该是三个字母的！*

#### 第16条：提供 “全能初始化方法（designed initializer）”

> * 在类中提供一个全能初始化方法，并于文档中指明。其他初始化方法均应调用此方法。
> * 若子类的全能初始化方法与超类不同，则需覆写超类中的对应方法。
> * 如果超类的初始化方法不适用于子类，那么应该覆写这个超类方法，并在其中抛出异常。


*`全能初始化方法`：指为对象提供必要信息以便其能完成工作的初始化方法。*

*如何抛异常 举个 🌰 ：*

```objc
@throw [NSException exceptionWithName:NSInternalInconsistencyException reason:@"这里写异常原因" userInfo:nil];
```

#### 第17条：实现description方法

> * 实现 description 方法返回一个有意义的字符串，用以描述该实例。

*如果想在description方法中输出很多互不相同的信息，那就借助NSDictionary类的description方法。 举个 🌰 ：*

```objc
- (NSString *)description
{
    return  [NSString stringWithFormat:@"<%@: %p %@>",[self class],self,@{@"title":_title,@"latitude":@(_latitude),@"longitude":@(_longitude)}];
}
```

> * 若想在调试时打印出更详尽的对象描述信息，则应实现 debugDescription 方法。

*debugDescription方法是开发者在调试器(debugger)中以控制台命令打印对象时才调用的。*

*LLDB 的 `po` 命令可以完成对象打印(print-object)工作！举个 🌰 ：*

```objc
- (NSString *)description
{
    return [NSString stringWithFormat:@"%@ %@",_firstName,_lastName];
}

- (NSString *)debugDescription
{
    return [NSString stringWithFormat:@"<%@: %p \" %@ %@ \">",[self class],self,_firstName,_lastName];
}
```

#### 第18条：尽量使用不可变对象

> * 尽量创建不可变的对象。

*举个 🌰 ：*

```objc
@property (nonatomic,copy,readonly) NSString * firstName; //!<  名字
```

> * 若某属性仅用于对象内部修改，则在 “class-continuation分类” 中将 readonly 属性扩展为 readwrite 属性。

*举个 🌰 ：*

```objc
@interface XYJPerson ()

@property (nonatomic,copy,readwrite) NSString * firstName;

@end
```

> * 不要把可变的 collection 作为属性公开，而应提供相应的方法，以此修改对象中的可变 collection。

*举个 🌰 ：*

```objc

//.h文件中的声明
@property (nonatomic,strong,readonly) NSSet * friends;

- (void) addFriends:(XYJPerson *) person;
- (void) deleteFriends:(XYJPerson *) person;

//.m文件中的实现
{
    NSMutableSet * _internalFriends;
}

- (instancetype)initWithFirstName:(NSString *) first lastName:(NSString *) last
{
    self = [super init];
    if (self) {
        
        _firstName = [first copy];
        _lastName = [last copy];
        
        _internalFriends = [NSMutableSet new];  //注意初始化
    }
    return self;
}

- (void)addFriends:(XYJPerson *)person
{
    [_internalFriends addObject:person];
}

- (void) deleteFriends:(XYJPerson *)person
{
    [_internalFriends removeObject:person];
}

//覆写friends的getter方法
- (NSSet *)friends   
{
    return [_internalFriends copy];
}

```

*不要在返回的对象上查询类型以确定其是否可变。*

*开发者或许不宜从底层直接修改对象中的数据。*

#### 第19条：使用清晰而协调的命名方式

> * 起名时应遵从标准的 Objective-C 命名规范，这样创建出来的接口更容易为开发者所理解。
> * 方法名要言简意赅，从左至右读起来要像个日常用语中的句子才好。
> * 方法名里不要使用缩略后的类型名称
> * 给方法起名时的第一个要务就是确保风格与你自己的代码或所集成的框架相符。


*变量与方法名使用 `驼峰式大小命名法` --- 以小写字母开头，其后每个单词首字母大写。类名也用驼峰命名法，不过其首字母要大写，前面通常还有三个前缀字母。*

**方法命名的注意事项：**

* *如果方法的返回值是新创建的，那么方法名的首个词应该是返回值的类型，除非前面还有修饰语。属性的存取方法不遵循这种命名方式，因为一般认为这些方法不会创建新对象，即便有时返回内部对象的一份拷贝，我们也认为那相当于原有的对象。这些存取方法应该按照其所对应的属性来命名。*
* *应该把表示参数类型的名词放在参数前面。 如：mutableString*
* *如果方法要在当前对象上执行操作，那么就应该包含动词；若执行操作时还需要参数，则应该在动词后面加上一个或多个名词。*
* *不要使用 str 这种简称，应该使用 string 这样的全称。*
* *Boolean 属性应加 is 前缀。如果某方法返回非属性的 Boolean 值，那么应该根据其功能，选用 has 或 is 当前缀。*
* *将 get 这个前缀留给那些借由“输出参数”来保存返回值的方法。*

#### 第20条：为私有方法名加前缀

> * 给私有方法的名称加上前缀，这样可以很容易地将其同公共方法区别开。
> * 不要单用一个下划线做私有方法的前缀，因为这种做法是预留给苹果公司用的。


#### 第21条：理解Objective-C错误类型

> * 只有发生了可使整个应用程序崩溃的严重错误时，才应使用`异常`。
> * 在错误不那么严重的情况下，可以指派`委托方法` (delegate method) 来处理错误，也可以把错误对象放到 `NSError` 对象里，经由`输出参数`返回给调用者。



#### 第22条：理解 `NSCopying协议`

> * 若想令自己所写的对象具有拷贝功能，则需实现 NSCopying 协议。
> * 如果自定义的对象分为可变版本和不可变版本，那么需要同时实现 `NSCopying` 和 `NSMutableCopying` 协议。
> * 复制对象时需决定采用深拷贝还是浅拷贝，一般情况下应该尽量执行浅拷贝。
> * 如果你写的对象需要深拷贝，那么可考虑新增一个专门执行深拷贝的方法。

*若想某个类支持拷贝功能，只需声明该类遵循NSCopying协议，并实现其中的 `copyWithZone` 这个方法。*

*举个 🌰 ：*

```objc
- (id) copyWithZone:(NSZone *) zone
{
    XYJPerson * copy = [[[self class] allocWithZone:zone] initWithFirstName:_firstName andLastName:_lastName];
    return copy;
}
```

*在可变对象上调用 copy 方法会返回另外一个不可变类的实例。*

*`深拷贝`：在拷贝对象自身时，将其底层数据也一并复制过去。*

*`浅拷贝`：只拷贝容器对象本身，而不复制其中数据，浅拷贝之后的内容与原始内容均指向相同对象。*


#### 第23条：通过委托与数据源协议进行对象间通信

> * 委托模式为对象提供了一套接口，使其可由此将相关事件告知其他对象。
> * 将委托对象应该支持的接口定义成协议，在协议中把可能需要处理的事件定义成方法。
> * 当某对象需要从另外一个对象中获取数据时，可以使用委托模式。这种情境下，该模式亦称“数据源协议（data source protocol）”。
> * 若有必要，可实现含有位段的结构体，将委托对象是否能相应相关协议方法这一信息缓存至其中。


*Objective-C不支持多重继承，因而我们把某个类应该实现的一系列方法定义在协议里面。协议最为常见的用途是实现委托模式。*

*`类别（Category）`使得我们无需继承子类即可直接为当前类添加方法。*

`委托模式（Delegate pattern）`的主旨是：

> 定义一套接口，某对象若想接受另一个对象的委托，则需遵从此接口，以便成为其委托对象 （Delegate）。而另一个对象则可以给其委托对象回传一些信息，也可以在发生相关事件时通知委托对象。

**被委托的对象的操作步骤：**

- 1、定义一套协议，协议名通常是在相关类名后面加上 Delegate 一词，然后声明协议要实现的方法，一般是可选的，用 `@optional` 关键字标注。
- 2、在类中声明一个委托对象的属性，需用 weak 来修饰。
- 3、事件触发的时候调用声明的协议方法。

**委托对象的操作步骤：**

- 1、一般在 `class-continuation` 分类中声明遵守该委托协议。
- 2、设置被委托对象的 Delegate 为 self。
- 3、实现委托协议中的代理方法。

#### 第24条：将类的实现代码分散到便于管理的数个分类（category）之中

> * 使用分类机制把类的实现代码划分成易于管理的小块。
> * 将应该视为“私有”的方法归入名为 Private 的分类中，以隐藏实现细节。


#### 第25条：总是为第三方类的分类名称加前缀

> * 向第三方类中添加分类时，总应给其名称加上你专用的前缀。
> * 向第三方类中添加分类时，总应给其中的方法加上你专用的前缀。

#### 第26条：勿在分类中声明属性

> * 把封装数据所用的全部属性都定义在主接口里。
> * 在“ class-continuation 分类”之外的其他分类中，可以定义存取方法，但尽量不要定义属性。

#### 第27条：使用“class-continuation分类”隐藏实现细节

> * 通过 `class-continuation 分类`向类中新增实例变量。
> * 如果某属性在主接口中声明为 `readonly` ，而类的内部又要用设置方法修改此属性，那么就在 `class-continuation分类` 中将其扩展为 `readwrite`。
> * 把私有方法的原型声明在 `class-continuation 分类`里面。
> * 若想使类所遵循的协议不为人所知，则可于`class-continuation 分类`中声明。

#### 第28条：通过协议提供匿名对象

> * 协议可在某种程度上提供匿名类型。具体的对象类型可以淡化成遵从某协议的 id 类型，协议里规定了对象所应实现的方法。
> * 使用匿名对象来隐藏类型名称（或类名）。
> * 如果具体类型不重要，重要的是对象能够响应（定义在协议里的）特定方法，那么可以使用匿名对象来表示。

#### 第29条：理解引用计数

> * 引用计数机制通过可以递增递减的计数器来管理内存。对象创建好之后，其保留计数至少为 1。若保留计数为正，则对象继续存活。当保留计数降为 0 时，对象就销毁了。
> * 在对象生命周期中，其余对象通过引用来保留或释放此对象。保留和释放操作分别会递增及递减保留计数。

*autorelease 能延长对象生命周期，使其在跨越方法调用边界后依然可以存活一段时间。*


#### 第30条：以ARC简化引用计数


> * 有 ARC 之后，程序员就无需担心内存管理问题了。使用 ARC 来编程，可省去类中的许多“样板代码”。
> * ARC 管理对象生命期的办法基本上就是：在合适的地方插入“保留”及“释放”操作，在ARC环境下，变量的内存语义可以通过修饰符指明，而原来则需要手工执行“保留”及“释放”操作。
> * 由方法所返回的对象，其内存管理语义总是通过方法名来体现。ARC 将此确定为开发者必须遵守的原则。
> * ARC 只负责管理 Objective-C 对象的内存。尤其要注意：CoreFoundation 对象不归 ARC 管理，开发者必须适时调用 `CFRetain` / `CFRelease`。


#### 第31条：在 dealloc 方法中只释放引用并解除监听

> * 在 dealloc 方法里，应该做的事情就是释放指向其他对象的引用，并取消原来订阅的键值观测（KVO）或 NSNotificationCenter 等通知，不要做其他事情。
> * 如果对象持有文件描述符等系统资源，那么应该专门编写一个方法来释放此种资源。这样的类要和其使用者约定：用完资源后必须调用 close 方法。
> * 执行异步任务的方法不应在 dealloc 方法里调用；只能在正常状态下执行的那些方法也不应在 dealloc 里调用，因为此时对象已处于正在回收的状态了。


#### 第32条：编写“异常安全代码”时留意内存管理问题

> * 捕获异常时，一定要注意将 try 块内所创立的对象清理干净。
> * 在默认情况下，ARC 不生成安全处理异常所需的清理代码。开启编译器标志后，可以生成这种代码，不过会导致应用程序变大，而且降低运行效率。

*若使用ARC且必须捕获异常，则需打开编译器的 `-fobjc-arc-exceptions` 标志。*

#### 第33条：以若引用避免保留环

> * 将某些应用设为 weak，可避免出现“保留环”。
> * weak 引用可以自动清空，也可以不自动清空。自动清空（ autoniling） 是随着 ARC 而引入的新特性，由运行期系统来实现。在具备自动清空功能的弱引用上，可以随意读取其数据，因为这种引用不会指向已经回收过的对象。

*`unsafe_unretained` 一词表明，属性值可能不安全，而且不归此实例所拥有。*
*`weak` 和 `unsafe_unretained`  的作用一样，唯一的不同是，只要系统把属性回收，属性值就会自动设置为 nil。但是 `unsafe_unretained ` 属性仍然指向那个已经回收的实例。*

#### 第34条：以“自动释放池”降低内存峰值

> * 自动释放池排布在栈中，对象收到 autorelease 消息后，系统将其放入最顶端的池里。
> * 合理运用释放池，可降低应用程序的内存峰值。
> * @autoreleasepool 这种新式写法能创建出更为轻便的自动释放池。

*自动释放池用于存放那些需要在稍后某个时刻释放的对象。清空自动释放池时，系统会向其中的对象发送release 消息。*

*内存峰值 （high-memory waterline） 是指应用程序在某个特定时间段内的最大内存用量 （highest memory footprint）。*

#### 第35条：用“僵尸对象”调试内存管理问题

> * 系统在回收对象时，可以不将其真的回收，而是把它转化为僵尸对象。通过环境变量 NSZombieEnabled 可开启此功能。
> * 系统会修改对象的 isa 指针，令其指向特殊的僵尸类，从而使该对象变为僵尸对象。僵尸类能够响应所有的选择子，响应方式为：打印一条包含消息内容及其接收者的消息，然后终止应用程序。

#### 第36条：不要使用retainCount

> * 对象的保留计数看似有用，实则不然，因为任何给定时间点上的”绝对保留计数”（absolute retain count）都无法反应对象生命期的全貌。
> * 引入 ARC 之后，retainCount 方法就正式废止了，在 ARC 下调用该方法会导致编译器报错。

#### 第37条：理解Block这一概念

> * 块是 C、C++、Objective-C 中的词法闭包。
> * 块可接收参数，也可返回值。
> * 块可以分配在栈或堆上，也可以是全局的。分配在栈上的块可拷贝在堆里。这样的话，就和标准的Objective-C 对象一样，具备引用计数了。

*块的语法结构如下：*

```objc
return_type (^block_name)(parameters)
```

举个 🌰 ：

```objc
int (^addBlock) (int a, int b) = ^(int a, int b) {
	return a+b;
}
```

*块的强大之处：在声明它的范围内，所有变量都可以为其捕获。如果要修改捕获的变量，那么，声明变量的时候需要加上 `__block` 关键词。*

#### 第38条：为常用的Block类型创建typedef

> * 以 typedef 重新定义块类型，可令块变量用起来更加简单。
> * 定义新类型时应遵守现有的命名习惯，勿使其名称与别的类型相冲突。
> * 不妨为同一个块签名定义多个类型别名。如果要重构的代码使用了块类型的某个别名，那么只需修改相应的typedef 中的块签名即可，无需改动其他 typedef。

* `typedef` 关键字用于给类型起个易读的别名。*

```objc
typedef double NSTimeInterval;
```

*Block 的 typedef:*

```objc
typedef int (^XYJSomeBlock) (BOOL flag, int value);
```

#### 第39条：用handle块降低代码分散程度

> - 在创建对象时，可以使用内联的 handle 块将相关逻辑一并声明。
> - 在有多个实例需要监控时，如果采用委托模式，那么经常需要根据传入的对象来切换，而若改用 handle 块来实现，则可直接将块与相关对象放在一起。
> - 设计 API 时如果用到了 handle 块，那么可以增加一个参数，使调用者可通过此参数来决定应该把块安排在哪个队列上。

#### 第40条：用块引用其所属对象时不要出现保留环

> * 如果块所捕获的对象直接或间接地保留了块本身，那么就得当心保留环问题。
> * 一定要找个适当的时机解除保留环，而不能把责任推给 API 的调用者。

#### 第41条：多用派发队列，少用同步锁

> * 派发队列可用来表述同步语义（synchronize semantic），这种做法要比使用 @synchronized 块或NSLock 对象更简单。
> * 将同步和异步派发结合起来，可以实现与普通加锁机制一样的同步行为，而这么做却不会阻塞执行异步派发的线程。
> * 使用同步队列及栅栏块，可以令同步更加高效。

#### 第42条：多用 GCD，少用 performSelector 系列方法

> * performSelector 系列方法在内存管理方面容易有疏失。它无法确定将要执行的选择子具体是什么，因而 ARC 编译器也就无法插入适当的内存管理方法。
> * performSelector 系列方法所能处理的选择子太过局限了，选择子的返回值类型及发送给方法的参数个数都受到限制。
> * 如果想把任务放到另一个线程上执行，那么最好不要用 performSelector 方法，而是应该把任务封装在块里，然后调用 GCD 的相关方法来实现。

#### 第43条：掌握 GCD 及操作队列的使用时机

> - 在解决多线程与任务管理问题时，派发队列并非唯一方案。
> - 操作队列提供了一套高层的 Objective-C API ,能实现纯 GCD 所具备的绝大部分功能，而且还能完成一些更为复杂的操作，那些操作若改用 GCD 来实现，则需另外编写代码。

*GCD是纯C的API，而操作队列则是 Objective-C 的对象。*

使用`NSOperation`和`NSOperationQueue`的好处：

* 1、取消某个操作。
* 2、指定操作间的依赖关系。
* 3、通过键值观察机制监控 NSOperation 对象的属性。
* 4、指定操作的优先级。
* 5、重用 NSOperation 对象。

#### 第44条：通过 Dispatch Group 机制，根据系统资源状况来执行任务

> * 1、一系列任务可归入一个 dispatch group 之中。开发者可以在这组任务执行完毕时获得通知。
* 2、通过 dispatch group，可以在并发式派发队列里同时执行多项任务。此时GCD会根据系统资源状况来调度这些并发执行的任务。开发者若自己来实现此功能，则需编写大量代码。

#### 第45条：使用dispatch_once来执行只需运行一次的线程安全代码

> * 1、经常需要编写“只需执行一次的线程安全的代码”（thread-safe single-code execution）。通过GCD 所提供的 `dispatch_once` 函数，很容易就能实现此功能。
> * 2、标记应该声明在 static 或 global 作用域中，这样的话，在把只需执行一次的块传给`dispatch_once` 函数时，传进去的标记也是相同的。

单例的使用：举个 🌰

```objc
+ (FFHttpRequestManager *)shareManager
{

    static FFHttpRequestManager *shareInstance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        shareInstance = [[FFHttpRequestManager alloc]init];
    });
    return shareInstance;

}
```

#### 第46条：不要使用dispatch_get_current_queue

> * `dispatch_get_current_queue` 函数的行为常常与开发者所预期的不同。此函数已经废弃，只应做调试只用。
> * 由于派发队列是按层级来组织的，所以无法单用某个队列对象来描述“当前队列这一概念”。
> `dispatch_get_current_queue` 函数用于解决由不可重入的代码所引发的死锁，然而能用此函数解决的问题，通常也能改用“队列特定数据”来解决。


#### 第47条：熟悉系统框架

> * 许多系统框架都可以直接使用。其中最重要的是 Foundation 与 CoreFoundation，这两个框架提供了构建应用程序所需的许多核心功能。
> * 很多常见任务都能用框架来做，例如音频与视频处理、网络通信、数据管理等。
> * 请记住：用纯C写成的框架与用 Objective-C 写成的一样重要，若想成为优秀的 Objective-C 开发者，应该掌握 C 语言的核心概念。


#### 第48条：多用块枚举，少用for循环

> * 遍历 collection 有四种方式。最基本的是 for 循环，其次是 NSEnumerator 遍历法及快速遍历法，最新最先进的方式则是“块枚举法”。
> * “块枚举法”本身就能通过 GCD 来并发执行遍历操作，无须另行编写代码。而采用其他遍历方式则无法轻易实现这一点。
> * 若提前知道待遍历的 collection 含有何种对象，则应修改块签名，指出对象的具体类型。

##### for 循环

*数组的遍历*

```objc
NSArray * anArray = @[@"one",@"two",@"three"];
for (int i = 0; i < anArray.count; i++) {
        
    id object = anArray[i];
    // Do something with 'object'
}
```

*字典的遍历*

```objc
NSDictionary * aDictionary = @{@"one":@"1",@"two":@"2",@"three":@"3"};
NSArray * keys = [aDictionary allKeys];
for (int i = 0; i < keys.count; i++) {
        
     id key = keys[i];
     id value = aDictionary[key];
     // Do something with 'key' and 'value'
}
```

*Set 的遍历*

```objc
NSSet * aSet = /* ... */;
NSArray * objects = [aSet allObjects];
for (int i = 0; i < objects.count; i++) {
    id object = objects[i];
    // Do something with 'Object'
}
```
##### NSEnumerator 来遍历

*数组的遍历*

```objc
NSArray * array = @[@"apple",@"banana",@"orange"];
NSEnumerator * enumerator = [array objectEnumerator];
id object;
while ((object = [enumerator nextObject]) != nil) {
     //Do something with 'object'
}
```
*数组的反向遍历*

```objc
NSArray * array = @[@"apple",@"banana",@"orange"];
for (id object in [array reverseObjectEnumerator]) {
        // Do something with 'object'
}
```

*字典的遍历*

```objc
NSDictionary * aDictionary = @{@"one":@"1",@"two":@"2",@"three":@"3"};
NSEnumerator * enumerator = [aDictionary keyEnumerator];
id key;
while ((key = [enumerator nextObject]) != nil) {
        
    id value = aDictionary[key];
    // Do something with 'key' and 'value'
}
```
*Set 的遍历*

```objc
NSSet * aSet = /* ... */;
NSEnumerator * enumerator = [aSet objectEnumerator];
id object;
while ((object = [enumerator nextObject]) != nil) {
        
    // Do something with 'object'
}
```

##### for-in 快速遍历

*数组的遍历*

```objc
NSArray * anArray = @[@"one",@"two",@"three"];
for (id object in anArray) {
        
     // Do something with 'object'
}
```
*字典的遍历*

```objc
NSDictionary * aDictionary = @{@"one":@"1",@"two":@"2",@"three":@"3"};
for (id key in aDictionary) {
        
     id value = aDictionary[key];
     // Do something with 'key' and 'value'
}
```
*Set 的遍历*

```objc
NSSet * aSet = /* ... */;
for (id object in aSet) {
        
    // Do something with 'object'
}
```

##### 基于块的遍历方式

*数组的遍历*

```objc
NSArray * array = @[@"apple",@"banana",@"orange"];
    [array enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
     
    //Do something with 'obj'
        
    //if stop
    *stop = YES;
        
}];
```
*字典的遍历*

```objc
NSDictionary * aDictionary = @{@"one":@"1",@"two":@"2",@"three":@"3"};
    [aDictionary enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        
    //Do something with 'key' and 'obj'
        
    //if stop
    *stop = YES;
        
}];
```
*Set 的遍历*

```objc
NSSet * aSet = /* ... */;
    [aSet enumerateObjectsUsingBlock:^(id  _Nonnull obj, BOOL * _Nonnull stop) {
        
    //Do something with 'obj'
        
    //if stop
    *stop = YES;
}];
```

#### 第49条：对自定义其内存管理语义的collection使用无缝桥接（toll-free bridging）

> * 通过无缝桥接技术，可以在 Foundation 框架中的 Objective-C 对象与 CoreFoundation 框架中的 C 语言结构之间来回转换。
> * 在 CoreFoundation 层面创建 collection 时，可以指定许多回调函数，这些函数表示此 collection 应如何处理其元素。然后，可运用无缝桥接技术，将其转换成具备特殊内容管理语义的 Objective-C collection。

*__bridge ARC 仍然具备这个 Objective-C 对象的所有权。*
*__bridge_retained ARC 将交出对象的所有权。*
*__bridge_transfer 类似于想把 CFArrayRef 转换为 NSArray ，并且想让 ARC 获得对象所有权。*

#### 第50条：构建缓存时选用 NSCache 而非 NSDictionary

> * 实现缓存时应选用 NSCache 而非 NSDictionary 对象。因为 NSCache 可以优雅的自动删减功能，而且是线程安全的，此外，它与字典不同，并不会拷贝键。
> * 可以给 NSCache 对象设置上限，用以限制缓存中的对象总个数及总成本，而这些尺寸则定义了缓存删减其中对象的时机。但是绝对不要把这些尺度当成可靠的“硬限制”，他们仅对 NSCache 起指导作用。
> * 将 NSPurgeableData 与 NSCache 搭配使用，可实现自动清除数据的功能，也就是说，当NSPurgeableData 对象所占内存为系统所丢弃时，该对象自身也会从缓存中移除。
> * 如果缓存使用得当，那么应用程序的响应速度就能提高。只有那种“重新计算起来很费事”的数据，才值得放入缓存，比如那些需要从网络获取或从磁盘读取的数据。


#### 第51条：精简 initialize 与 load 的实现代码

> * 在加载阶段，如果类实现了 load 方法，那么系统就会调用它。分类里也可以定义此方法，类的 load 方法要比分类中的先调用。与其他方法不同，load 方法不参与覆写机制。
> * 首次使用某个类之前，系统会向其发送 initialize 消息。由于此方法遵从普通的覆写规则，所以通常应该在里面判断当前要初始化的是哪个类。
> * load 与 initialize 方法都应该精简一些，这有助于保持应用的响应能力，也能减少引入“依赖环”的几率。
> * 无法在编译期设定的全局常量，可以放在 initialize 方法里初始化。

#### 第52条：别忘了 NSTimer 会保留其目标对象

> * NSTimer 会保留其目标，直到计时器本身失效为止，调用 invalidate 方法可令计时器失效，另外，一次性的计时器在触发完任务之后也会失效。
> * 反复执行任务的计时器，很容易引入保留环，如果这种计时器的目标对象又保留了计时器本身，那肯定会导致保留环。这种环状保留关系，可能是直接发生的，也可能是通过对象图里的其他对象间接发生的。
> * 可以扩充 NSTimer 的功能，用块来打破保留环。不过除非NSTimer将来在公共接口里提供此功能，否则必须创建分类，将相关的代码加入其中。

*感谢您的阅读，一起学习，一起成长，加油！*