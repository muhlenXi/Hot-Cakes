---
title: iOS BLE 开发小记[1] - CoreBluetooth 是什么
date: 2017-04-27 15:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/04/27/iOS-Bluetooth-Low-Energy-Develop-Chapter1](http://muhlenxi.com/2017/04/27/iOS-Bluetooth-Low-Energy-Develop-Chapter1)*

### 导语：

> 不知不觉从事 iOS 低功耗蓝牙开发也很长一段时间了，一直没时间来的及把自己关于这方面的学习和收获写下来，最近项目迭代更新上线完，有点业余时间，抓紧时间总结一下，说这是与同阶段的小伙伴们之间的交流也好，是给后来的小伙伴的掉坑经验也好。毕竟时间是挤出来的！做技术的人一般都有一个很开放的心态！ 

<!-- more -->

现在我们都知道，很多智能硬件设备都已经集成了低功耗蓝牙模块，这样我们就可以开发一个 iOS 或者 Mac APP 与它们进行交互。从 macOS 10.9 和 iOS 6 以后，Mac 和 iOS 设备就支持 低功耗蓝牙技术了，我们可以通过 `CoreBluetooth` 这个框架与底层的各种蓝牙协议栈进行交互，比如 GATT、ATT 和 L2CAP 等。

与底层交互的过程如下图所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/CBTechnologyFramework.png" width="160" height="235" alt="选项列表图"/>
</div>

开始下文之前，我们需要了解几个概念。对蓝牙不够了解的可以看一下维基百科关于[蓝牙](https://zh.wikipedia.org/wiki/%E8%97%8D%E7%89%99#.E4.BD.8E.E8.80.97.E9.9B.BB.E8.97.8D.E7.89.99BLE.EF.BC.88Bluetooth_Low_Energy.EF.BC.89)的简介。

`Bluetooth 4.0`: 蓝牙 4.0 是 Bluetooth SIG 于2010年7月7日推出的新的规范，其最重要的特性是功耗低，省电！

`BLE`: Bluetooth low energy wireless technology,也就是低功耗无线蓝牙技术。

### 简单了解 CoreBluetooth

#### BLE 是什么？

BLE 是关于蓝牙4.0 的详细说明，它定义了一套用于低功耗设备之间通信的协议。而CoreBluetooth 则是对 BLE 协议栈的抽象。也就是说，它隐藏了许多底层的详细实现细节，这样对我们开发者来说，开发一个 APP 与 BLE 设备进行交互将会很便捷。


#### 两个核心角色 Central 和 Peripheral

CoreBluetooth 中最关键的两个角色就是 Central(中心) 和 Peripheral（周边）, Peripheral 一般是提供数据的一方，而 Central 一般获取 Peripheral 提供的数据然后来完成特定的任务。举个例子，一个集成 BLE 的数字室温计可能提供房间中的实时温度，我们通过 APP 就可以读取、分析和显示房间中的温度。

Peripheral 通过向空中广播数据的方式来使我们能感知到它的存在。Central 通过扫描搜索来发现周围正在广播数据的 Peripheral, 找到指定的 Peripheral 后，发送连接请求进行连接，连接成功后则与 Peripheral 进行一些数据交互， Peripheral 则会通过合适的方式对 Central 进行响应。

#### CoreBluetooth 简化常见蓝牙任务

CoreBluetooth 对通用的蓝牙任务进行了简化处理，你在 App 中通过 CoreBluetooth 来集成 BLE 功能将会变得简单，如果你开发的 APP 遵循了 Centrals 的开发规范，CoreBluetooth 将会帮你处理与 Peripheral 的扫描、连接以及数据交互的过程，除此之外，通过 CoreBluetooth 将你的设备设置为 本地 Peripheral 也会很便捷。

#### iOS APP 的状态影响蓝牙的行为

iOS APP 的状态也会影响蓝牙的行为，当你的 APP 在后台运行或者处于暂停状态中，蓝牙的行为将会受到影响。默认情况下，当你的 APP 在后台运行时或者处于暂停状态中，你的 APP 是不能与 BLE 进行数据通信的，也就是说，当 APP 后台运行时，你需要与 BLE 进行数据通信，你需要声明你的 APP 支持蓝牙后台运行模式，即使你声明了支持后台运行模式，蓝牙在后台运行模式下的数据处理方式也会变得不同，当开发你的 BLE APP 时，你需要注意这些不同点。

即使 APP 在后台运行时，当系统内存过低时也会杀掉 APP 的后台进程，对于 iOS 7，CoreBluetooth 支持 Central 和 Peripheral 的状态信息的保存和恢复。可以通过这个功能来实现与 BLE 设备的长期交互。

#### 通过恰当的方式提高用户体验

CoreBluetooth 框架为你的 APP 与许多常见的 BLE 设备进行交互提供了交互接口，通过合理的利用和实践将会提高用户的体验。

举个例子，当你实现 Central 或 Peripheral 的功能时，会利用设备携带的无线电广播设备（Radio）向空中广播信号，这样就会影响到电池的续航时间，因此当你设计 APP 时，需要尽可能的减少 Radio 的使用频率。

### 深入 CoreBluetooth

重要提醒： *在 iOS 10以后，通过 CoreBluetooth 与 BLE 设备进行数据通信时，必须在项目的`Info.plist`文件中包含关于`NSBluetoothPerpheralUsageDescription`的描述,否则会导致 APP 闪退，详情见[NSBluetoothPerpheralUsageDescription](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW20)。*

#### Central 和 Peripheral 的通信方式

在 BLE 通信中主要包含两种角色：Central（中心）和 Peripheral（周边），基于传统的客户-服务器架构，Peripheral 通常会提供其他设备需要的数据，Central 通常利用通过 Peripheral 获取的信息来完成特定的任务，如图所示，心率监视器 提供数据给 Mac 或 iOS APP，然后来显示用户的心率数据。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/CBDevices1_2x.png" width="471" height="280" alt="选项列表图"/>
</div>


#### Central 搜索和连接正在广播数据的 Peripheral

Peripheral 以广播数据包的形式广播服务中的数据，广播数据包指的是包含 Peripheral 有用信息的一个较小数据包，比如 Peripheral 的名字和主要功能数据。比如，一个数字室温计广播的数据中可能包括当前室温，对于 BLE，广播是显示它们存在的主要方式。

如图，对于一个 Central 来说，它能够搜索和获取到它想要的 Peripheral 的广播信息。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/AdvertisingAndDiscovery_2x.png" width="344" height="48" alt="选项列表图"/>
</div>

#### Peripheral 是如何组织数据的

连接 Peripheral 的目的就是和 Peripheral 提供的数据进行交互，在你理解这一点后，可以更好的明白 Peripheral 的数据组成结构。

Peripheral 包含一个或多个 Service(服务)和连接信号强度的有用信息。Service 可以理解成是一个完成指定功能的数据集合。举个例子，一个心率监测服务的功能就是可能就是从心率传感器中读取心率数据。

Service 是由 Characteristic（特征） 组成的，Characteristic 为 Peripheral 的 Service 提供更详细的信息，举个例子，心率服务可能包含一个测量不同体位的心率数据的 Characteristic 和一个传输心率数据的 Characteristic，下图所示的是一个心率监测设备的数据组成结构。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/CBPeripheralData_Example_2x.png" width="172" height="300" alt="选项列表图"/>
</div>

#### Central 与 Peripheral 的数据交互

当 Central 与 Peripheral 建立成功的连接后，Central 可以发现 Peripheral 提供的全系列的 Service 和 Characteristic，广播数据包中的数据仅仅是可用服务的一小部分而已。

Central 可以通过读取或写入 Service Characteristic 值的方式与 Service 进行交互。你的 APP 也许需要从数字室温计中获取当前室内的温度或者设置一个温度值到数字室温计中。

### 如何表示 Central、Peripheral和 Peripheral 数据

BLE 通信过程中涉及到的主要角色和数据处理已经简单的集成到 CoreBluetooth 框架中了。

#### Central 方面的对象

当你通过本地 Central 与周围 Peripheral 进行交互时，你只需要调用 Central 方面的方法就可以了，除非你设置一个本地 Peripheral，并用它来响应其他的 Central 的交互请求，实际运用中，你的蓝牙处理大部分会在 Central 方面。 

##### Local（本地） Central 和 Remote（远程） Peripheral

在 Central 方面，用 `CBCentralManager` 对象来表示一个Local Central 设备，这个对象被用来管理 Remote Peripheral 设备（用 `CBPeripheral` 对象来表示），包括搜索和连接正在广播数据的 Peripheral。如图所示的是 CoreBluetooth 框架中如何表示 Local Central 和 Remote Peripheral。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/CBObjects_CentralSide_2x.png" width="441" height="195" alt="选项列表图"/>
</div>

##### 用 CBService 和 CBCharacteristic 对象来表示 Peripheral 中的服务数据

当你与 Remote Peripheral 进行数据交互时，你将处理它的 Service 和 Characteristic，在 CoreBluetooth 框架中，用 `CBService` 对象来表示 Peripheral 中的服务，同样地，用 `CBCharacteristic` 对象来表示 Service 中的特征。下图所示的是 Remote Peripheral 的服务特征结构树。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/TreeOfServicesAndCharacteristics_Remote_2x.png" width="275" height="253" alt="选项列表图"/>
</div>

#### Peripheral 方面的对象

对于 macOS 10.9 和 iOS 6， Mac 和 iOS 设备可以实现 BLE Peripheral 的功能，如为其他设备（包括 Mac，iPhone，和 iPad）提供数据。当你遵循 Peripheral 的开发规范时，就可以调用 BLE 通信的 Peripheral 方面的方法。

##### Local（本地） Peripheral 和 Remote（远程） Central

在 Peripheral 方面，一个 Local Peripheral 可以用 `CBPeripheralManager` 对象来表示，这个对象被用来管理发布包含的服务，包括组织构建 Peripheral 的数据结构以及向中心设备广播数据，Peripheral Manager 也对 Remote Central的读写交互请求做出响应。如图所示的是一个 Local Peripheral 和 Remote Central。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/CBObjects_PeripheralSide_2x.png" width="537" height="183" alt="选项列表图"/>
</div>

##### 用 CBMutableService 和 CBMutableCharacteristic 对象表示 LocalPeripheral 的数据

当你设置并与 Local Peripheral 进行数据交互时，你处理的是它的可变的 Service 和 Characteristic，在 CoreBluetooth 框架中，用 `CBMutableService` 对象来表示 Local Peripheral 中的服务，同样地，用 `CBMutableCharacteristic` 对象来表示Local Peripheral 服务中的特征。下图表示的是一个 Local Peripheral 中的服务特征结构树。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/TreeOfServicesAndCharacteristics_Local_2x.png" width="275" height="192" alt="选项列表图"/>
</div>

### 参考文献

1、[About Core Bluetooth](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/AboutCoreBluetooth/Introduction.html#//apple_ref/doc/uid/TP40013257-CH1-SW1)

2、[CoreBluetoothOverview](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/CoreBluetoothOverview/CoreBluetoothOverview.html)

### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*