---
title: iOS 多线程详细总结
date: 2017-08-22 14:21:46
tags: [pThread, NSThread, GCD, NSOperation]
---

本文是 muhlenXi 原创文章，欢迎转载，转载请注明出处: <http://muhlenxi.com/2017/08/22/multithreading>

## Overview

> 本文主要是关于 iOS 多线程开发中的知识总结，以及在实际项目中的用法和注意事项。

Stay Hungry, Stay Foolish.

<!-- more -->

## iOS 多线程

通过本文，你可以学到以下知识：

* 1、进程、线程 是什么？
* 2、多线程实现原理
* 3、多线程在 iOS 开发中的优缺点
* 4、多线程实现技术方案一 -- pThread
* 5、多线程实现技术方案二 -- NSThread
* 6、多线程实现技术方案三 -- GCD
* 7、多线程实现技术方案四 -- NSOperation
* 8、线程池的相关知识

*本文中使用的 Demo 已经传到 GitHub 上了，有需要的话，可以 Star 进行收藏。* [传送门](https://github.com/muhlenXi/SwiftEx/tree/master/multithreading)

下图所示是本文 demo 以及简介：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/MultithreadScreen.png" width="414" height="736" alt="选项列表图"/>
</div>

如图：

* 蓝色区域是 pThread 相关概念的实践。
* 橙色区域是 NSThread 相关概念的实践。
* 绿色区域是 Grand Central Dispatch 相关概念的实践。
* 红色区域是 NSOperation 相关概念的实践。

### 项目中用到多线程的场景

* 网络请求
* 图片加载
* 文件处理
* 数据存储
* 其他任务的执行

### 任务执行方式

* `串行`，多个任务顺序执行。
* `并行`，多个任务并发执行，也就是同时执行。

### 进程是什么？

`进程` 指的就是系统中正在运行的一个应用程序，它有*新建*、*就绪*、*运行*、*阻塞*和*终止*五种状态。

### 线程是什么？

`线程` 是进程中的基本执行单元，进程中的所有任务都是在线程中执行的。

### 多线程实现原理？

采用的是 `时间片轮转调度` 的方式来实现的。[更多了解](https://en.wikipedia.org/wiki/Multithreading)

### 多线程在 iOS 开发中的优缺点

优点：

* 简化了编程模型
* 更加的轻量级
* 提高了执行效率
* 提高了资源利用率

缺点：

* 增加了程序设计复杂性
* 占用内存空间
* 增大 CPU 调度开销

## 多线程技术方案

### pThread

描述：是基于 C 语言实现的框架，是一套通用的多线程 API，适用于 Unix/Linux/Windows 等系统，可以跨平台、移植，使用难度大，一般几乎不用。

使用语言：C 语言

线程生命周期：由程序员进行管理。

### NSThread

描述：对 C 语言框架进行了封装，完全是面向对象的，可直接操作线程对象。

使用语言：Objective-C

线程生命周期：由程序员进行管理。

#### 对象初始化

主要有 3 种方式来实现多线程：

* 方式1：通过 alloc 方式创建 NSThread 对象
* 方式2：通过 detachNewThreadSelector 方式
* 方式3：通过 performSelectorInBackground 方式

#### Set name

其中 *方式1* 创建的 **thread 对象**，可以设置线程的 name ,用来作为线程的标识，一般有以下用途：

* 1、用于 Debug 调试。
* 2、可以根据不同的线程来执行不同的业务逻辑。

#### 设置优先级

**thread 对象**还可以通过 `setThreadPriority`方法来设置不同的优先级。

终端多线程输出如下图所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/nsthread.png" width="899" height="191" alt="选项列表图"/>
</div>

在图中，我们不难发现：

* 红色箭头指的是 App 的进程 ID 2422
* 绿框中的指的是 主线程的 ID 120606
* 蓝色的框分别是新建的子线程，他们线程ID 依次是 120987，120988

#### 不同线程之间的资源竞争问题

在实际项目中经常有以下场景：多个线程同时设置或读取同一个 property 的值。这样会导致资源竞争，数据不准确的问题。

在 Demo 中我们通过抢红包游戏来模拟资源竞争问题，一共有3个红包，四个人同时开抢，如下图是没有做任何处理的情况下，四个线程同时抢红包，导致红包数量出错，因为他们几乎同时修改了红包的数量。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/beforeLock.png" width="976" height="108" alt="选项列表图"/>
</div>

我们可以通过 **线程加锁** 的方式来解决资源冲突问题。常用的有三种加锁方式，分别是

* @synchronized (self) { 在这里修改数据 }
* 通过 NSCondition 对象，修改数据前加锁，修改数据后解锁。
* 通过 NSLock 对象，修改数据前加锁，修改数据后解锁。

加锁后的数据如图所示，这次正常了，可怜的 muhlenXi 同学没抢到红包。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/afterLock.png" width="914" height="86" alt="选项列表图"/>
</div>

### GCD

描述：Grand Central Dispatch 是 Apple 公司针对多核并行运算提出的一套线程机制，GCD 会自动的管理线程的生命周期，使用起来比较方便和灵活。

使用语言：C 语言

生命周期管理：自动管理

#### 进程同步 和 进程异步

* `进程同步`：进程中的多个任务必须一个一个来处理，前一个任务处理完毕，才会进行下一个任务。同步会阻塞当前线程。
* `进程异步`：不用等待任务处理完毕，就可以执行下一个任务了。异步不会阻塞当前线程。

#### dispatch_get_global_queue

`dispatch_get_global_queue(0, 0)` 方法获取的是全局并发队列，第一个参数用于设置队列的优先级，使用场景如下：

* 异步执行不同任务

#### dispatch_get_main_queue

`dispatch_get_main_queue()` 方法获取的是 主线程队列，使用场景如下：

* 刷新 UI 

##### 注意事项

* 1、当有多个任务异步执行时，给 dispatch_get_global_queue 设置优先级，并不能准确保证每个任务的执行顺序，优先级高的线程仅仅说明在该线程执行任务的概率大些而已。

* 2、自定义一个同步线程队列(SERIAL)，然后把多个任务放入同一个线程中执行，方可保证任务的执行顺序。

#### dispatch_group_t

`dispatch_group_t` 使用场景如下：

* 在 *同一个同步子线程* (即自定义 serial queue) 中，多个任务异步执行处理后，需要统一的回调通知来得知所有的任务都处理完了，然后再执行相应的业务处理。

`dispatch_group_enter` 和 `dispatch_group_leave` 使用场景：

* 在 *同一个异步子线程* (即自定义 concurrent queue) 中发送多个异步数据请求，多个数据请求完成后，统一回调刷新 UI 等场景。

##### 注意事项

`dispatch_group_notify` 方法中，回调回来的数据是在异步线程中执行的，需要回到主线程中刷新 UI。

#### dispatch_once

`dispatch_once` 使用场景：

* 单例，App 声明周期中只会实例化一次。
* 整个生命周期只需要执行一次的代码块。

#### dispatch_after

`dispatch_after` 使用场景：

* 延迟执行某个任务。

##### 注意事项

当 ViewController 被 dealloc 后，如果时间到了，block 中的代码还会执行，会引起闪退问题，我们应该加以判断。

### NSOperation

描述:

NSOperation 是针对 GCD 封装的一个多线程抽象基类，不能直接被使用，需要使用其子类。子类主要有以下三种使用方式：

* 使用 NSInvocationOperation ,系统封装的。
* 使用 NSBlockOperation, 系统封装的。
* 自定义 Operation,需继承于 NSOperation。

使用语言：Objective-C
线程生命周期：自动管理

#### NSOperation 子对象

NSOperation 子对象有 5 种状态，分别是：

`ready (就绪)`、 `cancelled（取消）`、 `executing（执行)`、`finished（完成）` 和 `asynchronous（是否并发）`。

*实际项目中应该根据不同状态来执行不同业务逻辑。*

#### 添加依赖

NSOperation 子对象之间可以通过 `addDependency` 方法来添加依赖关系，注意别导致依赖环。

比如：A 依赖于 B，表明 B 任务执行完，才会执行 A 任务。

```objc
[blockOperA addDependency:blockOperB];
```

*注意事项：NSOperation 子对象的 start 方法使任务都会在当前线程中同步执行，会阻塞当前线程。*

#### NSOperationQueue 是什么？

`NSOperationQueue` 指的是操作队列，我们可以理解为线程池，我们可以将 Operation 对象加入到线程池中，操作系统会根据资源来调度。对于 NSOperationQueue ：

* 可以通过 `addOperation` 方法添加对象。
* 可以通过 `setMaxConcurrentOperationCount` 方法来设置线程池的最大并发数。

##### 注意事项

当我们自定义 Operation 执行异步网络请求任务时，如果此时有依赖，会导致依赖失效，因为异步请求会新开辟一个线程去执行任务，Operation 此时会认为，该任务已经完成，就会按照依赖关系去执行下一个任务。针对这种情况，解决办法就是：

第一步：在 自定义Operation 类中声明一个属性，用来表示线程是否结束；

```objc
@property (nonatomic,assign) BOOL  isOver;  //!< 线程是否结束
```

第二步：在 main 方法中使用 NSRunLoop 的方式使线程一直执行，直到异步网络请求任务完成后，再改变 isOver。

```objc
// 重写 main 方法执行自定义的任务
- (void)main {
	// 模拟网络请求
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        
        NSLog(@"执行模拟网络请求任务");
        [NSThread sleepForTimeInterval:1];
        
        if (self.isCancelled) {
            return;
        }
        
        NSLog(@"%@",self.name);
        self.isOver = YES;
    });
    
    // 使线程一直运行
    while (!self.cancelled && !self.isOver) {
        [[NSRunLoop currentRunLoop] runMode:NSDefaultRunLoopMode beforeDate:[NSDate distantFuture]];
    }
}
```

iOS 多线程到此就完结了。

## Ending

*如果觉得本文不错的话，不妨赞助一罐百事可乐，支持作者后期产出更多优质内容！*

*欢迎留言一起交流...*

