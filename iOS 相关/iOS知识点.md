#### 什么是UI?

UI即User Interface（用户界面）的简称。UI设计则是指对软件的人机交互、操作逻辑、界面美观的整体设计。好的UI设计不仅是让软件变得有个性有品味，还要让软件的操作变得舒适、简单、自由、充分体现软件的定位和特点。

#### UIViewController的生命周期

   	1.alloc创建对象,分配空间
   	2.init初始化对象,初始化数据
   	3.self.view 什么时候创建  — loadView
   	4.viewDidLoad作用
   	5.viewWillAppear函数
   	6.viewDidAppear函数
   	7.viewWillDisappear函数
   	8.viewDidDisappear函数
   	9.dealloc视图被销毁
   	
#### init（）的继承

* 1、如果派生类没有定义任何 designated initializer，那么它将走动继承所有基类的 designated initializer 。
* 2、如果一个派生类定义了所有基类的 designated initializer，那么它将继承基类所有的 convenience initializer。

*一个类所有的convenience init最终必须调用自己类的designated init 来初始化对象。*

*派生类的designated init 最终要调用基类的designated init 来初始化继承关系中基类的部分。*

参考[深入OOP - 继承和多态](https://www.boxueio.com/series/build-your-own-type-ii/ebook/34)。

技术是需要沉淀的。

