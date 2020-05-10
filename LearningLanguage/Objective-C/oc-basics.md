##### 1、说说你对 @class 、#include、#import 的理解？

`@class 类名` 向前声明某个类，有以下作用：

* 告诉编译器有一个叫 `类名` 的类。
* 可以减少类的使用者所引入的头文件的数量。
* 可以解决两个类互相引用的问题。

*--- Effective Objective-C 2.0 Second*

##### 2、 init 方法的继承原则是什么？

* 1、如果派生类没有定义任何 designated initializer，那么它将主动继承所有基类的 designated initializer 。
* 2、如果一个派生类定义了所有基类的 designated initializer，那么它将继承基类所有的 convenience initializer。

*一个类所有的 convenience init 最终必须调用自己类的designated init 来初始化对象。*

*派生类的 designated init 最终要调用基类的 designated init 来初始化继承关系中基类的部分。*

##### 3、如何理解 Objective-C 的消息传递和转发机制？

当我们调用对象的方法时，就是向这个对象传递消息，因为要调用的函数直到运行期才能确定，所以会使用动态绑定机制来决定需要调用的方法。编译器会将消息（receiver + selector + 参数）转换为标准的 C 语言调用（objc_msgSend）, objc_msgSend 会在 receiver 所属的类中搜索方法列表，如果能找到与 selector 名称相符的方法，就跳转到其实现代码。如果找不到，就沿着继承体系继续向上寻找，找到合适的方法再跳转。如果最终找不到相符的方法。则会执行 *消息转发* 操作。

*当对象接收到无法解读的消息后，就会启动消息转发机制（message forwarding ）。*

消息转发第一阶段会先征询 receiver 所属的类，看其能否能动态添加方法来处理这个 unknow selector，也就是 动态方法解析（dynamic method resolution）。

第二阶段会让 receiver 查阅是否有其他对象能处理这条消息，如果有，则把消息转给那个对象，消息转发过程到此结束。如果没有，则把与消息的全部细节封装到 NSInvocation 对象中，再给 receiver 一次机会，让其解决当前还未处理的消息。

#####  4、如何理解 @property、@synthesizer 和 @dynamic？

*property(属性),用于封装对象中的数据，通过 property 可以使编译器自动为我们生成变量的 setter 和 getter 方法，同时自动向类中添加 _属性名 的实例变量。*

*@synthesizer 用来指定生成的实例变量的名字。*

    @synthesize age = _age; // 指定生成的变量名为 _age,自己实现变量的 setter 和 getter 方法时需要指定，只实现其中的一个存取方法时不需要指定。


*@dynamic 告诉编译器不要自动创建属性所用的实例变量和合成 setter 和 getter 方法。*

**Attribute of property：**

    @property (原子性,读写权限,内存管理语义) NSString * name;

`atomic` : *原子性，编译器合成的方法会通过锁定机制来确保其原子性。*

`nonatomic`: *非原子性。*

`readwrite`: *可读可写，编译器会自动合成 setter 和 getter 方法。*

`readonly`: *只读，编译器只会自动合成 getter 方法。*

`assign`: *设置方法 只会执行 scalar type 的简单赋值操作。*

`strong`: *表明一种拥有关系，设置方法设置新值时，会先保留新值，并释放旧值，然后再将新值设置上去。*

`weak`: *表明一种非拥有关系，设置方法设置新值时，既不保留新值也不释放旧值，当属性所指的对象被摧毁时，属性值将置为 nil。*

`unsafe_unretained` : *和 assign 相同。只不过是针对 object type 的，表明一种非拥有关系，当对象被摧毁时，属性值 不会清空。*

`copy`: *类似 strong，不过设置方法设置新值时，不保留新值，而是将其 copy。*

`getter=name`: *指定 getter 的方法名。*

`setter= name`: *指定 setter 的方法名。*

##### 5、如何理解 Swift 中的方法派发机制？

*Swift 的方法派发机制主要有3种，第一种是 Direct Dispatch，第二种是 Table Dispatch，第三种是 Message Dispatch。*

派发规则：

*1-值类型永远使用 Direct Dispatch。*

*2-在 protocol 和 class 的定义中声明的方法使用 table dispatch。*

*3-在 protocol 和 class 的 extension 中定义的方法使用 direct dispatch。*

修改方法派发规则的 modifiers:

*如果你的方法需要被 Objective-C 运行时识别，需要用 `dynamic` 将方法的派发方式改为 message dispatch，此外还必须 import Foundation，它包含了 Objective-C 运行时的核心组件。*

*`@objc`仅仅可以让一个 Swift 方法被 Objective-C 运行时识别和访问，但不会修改该方法的派发方式。*

*`final` 用于把方法的派发方式改为 direct dispatch。*

*`@nonobjc` 仅仅让一个方法对 Objective-C 运行时不可见，但不会修改方法的派发方式。*

##### 6、什么情况使用 weak 关键字，相比 assign 有什么不同？

*1、在 ARC 中，在有可能循环引用的时候，往往要通过让其中一端使用 weak 来解决，比如 Delegate 属性。*

*2、自身已经对它进行一次强应用，没有必要再强引用一次，此时也会用 weak，自定义 IBOutlet 控件属性一般也使用 weak；当然也可以使用 strong。*

不同点：

*1、weak 表明该属性定义了一种“非拥有关系”。为这种属性设置新值时，设置方法既不保留新值，也不释放旧值。此特质和 assign 类似，然而当属性所指的对象遭受到摧毁时，属性值也会清空。而 assign 的设置方法只会执行针对纯量类型（如CGFloat、NSInteger）的简单赋值操作。*

*2、assign 可以用非 OC 对象，而 weak 必须用于 OC 对象。*
