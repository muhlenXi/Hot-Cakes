#### init（）方法的继承原则是什么？

* 1、如果派生类没有定义任何 designated initializer，那么它将走动继承所有基类的 designated initializer 。
* 2、如果一个派生类定义了所有基类的 designated initializer，那么它将继承基类所有的 convenience initializer。

*一个类所有的convenience init最终必须调用自己类的designated init 来初始化对象。*

*派生类的designated init 最终要调用基类的designated init 来初始化继承关系中基类的部分。*

参考[深入OOP - 继承和多态](https://www.boxueio.com/series/build-your-own-type-ii/ebook/34)。