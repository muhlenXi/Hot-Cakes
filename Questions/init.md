#### init（）方法的继承原则是什么？

* 1、如果派生类没有定义任何 designated initializer，那么它将主动继承所有基类的 designated initializer 。
* 2、如果一个派生类定义了所有基类的 designated initializer，那么它将继承基类所有的 convenience initializer。

*一个类所有的 convenience init 最终必须调用自己类的designated init 来初始化对象。*

*派生类的 designated init 最终要调用基类的 designated init 来初始化继承关系中基类的部分。*

参考来自 [泊学]() 的 [什么是 two-phrase initialization](https://www.boxueio.com/series/understand-ref-types/ebook/176)。

欢迎订阅成为**泊学会员**来查阅更多参考资料，通过 <https://www.boxueio.com/register/a7d248cb61146715c0dae59c7ed01187> 来订阅将会有惊喜哦！