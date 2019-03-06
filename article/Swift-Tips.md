---
title: Swift开发者必备Tips - 学习笔记
date: 2016-10-26 12:08:31
categories: Swift
tags: [学习笔记,Swift开发者必备Tips]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/10/26/Swift-Tips](http://muhlenxi.com/2016/10/26/Swift-Tips)*

### 前言：

> 这本书是王巍的关于Swift语言的知识点的集合，是作者学习和实践Swift的一些心得，作者以一个个的小技巧和知识点的形式，编写成了这本书，是中级开发人员的Swift进阶读本，该书涵盖了一个中高级开发人员需要知道的Swift语言的方方面面。

点击阅读全文来了解一下详情吧。

<!-- more -->

#### 正文

*柯里化（Currying）就是把接收多个参数的方法进行一些变形，使其更加灵活的方法。*

*Swift的`mutating`关键字修饰方法是为了能在该方法中修改`struct`或`enum`的变量。*

*@autoclosure做的事情就是把一句表达式自动地封装成一个闭包（closure）*

*`??`操作符可以快速地对nil进行条件判断，当??左侧的值为nil时返回右侧的值，否则返回左侧的值。*

```objc
let level:Int? = nil
let startLevel = 1
let currentLevel = level ?? startLevel
print(currentLevel)  //output 1
```

*Swift中我们可以定义一个接受函数作为参数的函数，而在调用时，使用闭包的方式来传递这个参数是常见手段。*

*函数中的参数（是函数）在异步线程中操作的时候，我们需要在block的类型前加上`@escaping`标记来表明这个闭包是会逃逸出该方法的。*