## 深入解析 MAC OS & iOS 系统

架构分层：

|层次| OS X | iOS |
|:---:|:---:|:---:|
|用户体验层| Aqua、Dashboard、Spotlight、accessibility| SpringBoard、Spotlight |
|应用程序框架| Cocoa、Carbon、Java | Cocoa Touch|
|核心框架|OpenGL QuickTime||
|Darwin 库和系统调用|内核、UNIX shell 环境||
|Mach 抽象|||
|硬件|||

------

**Darwin** 是苹果发布的一个开源操作系统，是 mac OS 和 iOS 系统的核心。

**bundle** 是一种标准化的层次结构，保存了可执行代码以及代码所需要的资源。

**系统调用** 指的是由内核导出的预定义函数的入口点，在用户态需要链接 /usr/lib/libSystem.B.dylib 才能访问这些调用。

**POSIX** (Portable Operating System Interface), 可移植操作系统接口，是一套定义了 *系统调用原型*、*系统调用编号*的标准的 API。

Darwin 的 **内核XNU** 的组件构成：

- Mach 微内核
- BSD 层
- libKern
- IO Kit

Mach 最基本职责：

* 进程和线程抽象
* 虚拟内存管理
* 任务调度
* 进程间通信和消息传递机制

每一个正在运行的应用程序实例都构成了一个**进程**，进程往往都是多线程的。
