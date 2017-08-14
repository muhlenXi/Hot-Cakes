#### 0、如何理解 @property、@synthesizer 和 @dynamic？

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
