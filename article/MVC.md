---
title: 对 iOS 开发中 MVC 模式的理解
date: 2016-02-04 10:41:11
categories: blog
tags: [MVC]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/02/04/MVC](http://muhlenxi.com/2016/02/04/MVC)*

#### 前言：

> MVC，全名是 Model View Controller，是模型 (model)－视图 (view)－控制器 (controller) 的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

学习贵在记录和总结收获！点击阅读全文了解更多！　　

<!-- more -->

#### 正文：

#### MVC 是什么？

*MVC 是一个基本机制，用于将程序中的所有对象拆分到三个阵营（三层）的一个阵营中。第一层是 Model，第二层是 View，第三层是 Controller。*

Model = 你的应用是什么？

Controller = 控制 Model 如何显示在屏幕上。

View = 你的控制器的元素，用于构成界面。

#### MVC 是如何通信的？

*Controller -> Model*

> Controller 对 Model 有完全访问权限。

*Model -> Controller*

> Model 通过 `Notification & KVO` 的方式与 Controller 通信。

*Controller -> View*

> *Controller 对 View 也有完全的访问权限。*如：Controller 拥有一个 outlet 属性，该属性指向View 中的对象。

*View -> Controller*

> *View 通过 `action-target` 的方式与 Controller 通信。*如：button 的点击

> *View 还通过 `Delegate` 的方式与 Controller通信。*

> *数据不能作为视图的内部属性。它是通过 `data source delegate` 的方式与 Controller 通信的。*也就是说，Controller 从 Model 中获取数据然后传递给 View。

*Model <-> View*

> *Model 和 View 不能相互通信。是完全独立的。*

通过下图，我们可以很好的理解他们之间的通信方式。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/mvc.PNG" width="568" height="320" alt="选项列表图"/>
</div>

#### 多个MVC的协作

MVC 的堆叠可以构成一个复杂的应用 如图所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/mav_together.PNG" width="568" height="320" alt="选项列表图"/>
</div>

*感谢您的阅读，一起学习，一起成长，加油！*

