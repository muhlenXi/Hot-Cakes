---
title: iOS BLE 开发小记[4] - 如何实现 CoreBluetooth 后台运行模式
date: 2017-05-03 10:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/05/03/iOS-Bluetooth-Low-Energy-Develop-Chapter4](http://muhlenxi.com/2017/05/03/iOS-Bluetooth-Low-Energy-Develop-Chapter4)*

### 导语：

> 在这一节，主要是 iOS APP 关于蓝牙后台处理方面的知识和经验。

<!-- more -->

对于 iOS APP 来说，知道你的 APP 是运行在前台还是运行在后台很重要。一个 APP 在后台运行状态下的行为表现必须不同于前台，因为 iOS 设备的系统资源是有限的。关于 iOS 后台运行处理的更多论述 请查阅 [Background Execution](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html#//apple_ref/doc/uid/TP40007072-CH4).

默认情况下，当你的 APP 在 background（后台运行）或者处于 suspended state（暂停状态）时，无论是 Central 端还是 Peripheral 端，许多常见的 CoreBluetooth 任务是不能被执行的。也就是说，你可以声明你的 APP 支持 CoreBluetooth 后台运行模式来允许你的 APP 进入 suspended state 后依然能够唤醒来处理一些蓝牙的事件。即使你的 APP 不需要全面的后台处理支持，但是当有重要事件发生的时候也需要系统给你发出弹框提醒。

即使你的 APP 不管支持哪一个 CoreBluetooth 后台运行模式，它也不能一直在后台运行。在某个时刻，系统会终止你的 APP 来为处于前台的其他 APP 提供内存空间，从而导致一些活动或者连接丢失。比如，对于 iOS 7， CoreBluetooth 支持 Central Manager 和 Peripheral Manager 对象状态信息的保存和在 APP 启动时恢复该状态信息，你可以使用这个功能来支持蓝牙设备的长期操作。

#### 只能前台运行的 APP

除非你请求允许执行特定的后台任务，否则对于大多数 iOS APP 来说，当你的 APP 进入 background state（后台状态）不久后就会处于 suspended state（暂停状态）。当你的 APP 处于 suspended state 时是不能执行蓝牙相关的任务，也不能感知到任何蓝牙事件直到重新进入 foreground（前台）。

在 Central 端，没有声明支持 CoreBluetooth 后台运行模式的 APP，也就是只能前台运行的 APP，在 background 和 suspended 状态时，是不能搜索正在广播数据的 Peripheral。在 Peripheral 端，则是不能进行广播数据。此时如果一个 Central 尝试获取 Peripheral Characteristic 的值会收到一个错误。

根据使用情况，这种默认行为可能会以多种方式影响你的 APP，举个例子，试想一下，当你正在与刚连接的 Peripheral 进行数据交互时，此时你的 APP 进入 suspended 状态（当用户切换到另一个 APP时）中，如果 Peripheral 此时失去连接，你是不能感知到已经发生 disconnection（断开连接）事件了，直到你的 APP 重新进入 foreground 为止。


##### 使用 Peripheral Connection options（选项）

只在前台运行的 APP 处于 suspended 状态时，系统会将发生的所有蓝牙事件加入队列，只有当 APP 重新进入前台时，才会将事件传递给 APP，也就是说，当 Central 产生事件时，CoreBluetooth 通过弹框提醒的方式通知用户。用户可以通过这个提醒来决定是否将 APP 重新打开来处理这个特定事件。

当调用 `CBCentralManager ` 类的 `connectPeripheral:options:` 方法连接 Remote Peripheral 时,你可以利用下面这些 Peripheral 连接选项来设置弹框提醒。

* `CBConnectPeripheralOptionNotifyOnConnectionKey`  -- 如果建立了一个成功的连接，此时 APP 进入 suspended 状态时，如果你想要系统为给定 Peripheral 显示一个弹框，你可以在 options 选项中包含这个键。
*  `CBConnectPeripheralOptionNotifyOnDisconnectionKey` -- 如果发生了 disconnection 事件，此时 APP 进入 suspended 状态时，如果你想要系统为给定 Peripheral 显示一个弹框，你可以在 options 选项中包含这个键。
*  `CBConnectPeripheralOptionNotifyOnNotificationKey` -- 如果收到了 Peripheral 的通知，此时 APP 进入 suspended 状态时，如果你想要系统显示一个来自 Peripheral 的通知弹框，你可以在 options 选项中包含这个件。

关于 Peripheral 连接参数的更多信息可以查阅 [Peripheral Connection Options ](https://developer.apple.com/reference/corebluetooth/cbcentralmanager) 常量。

### CoreBluetooth 后台运行模式

如果你的 APP 在后台运行状态下也需要执行蓝牙任务，你必须在 `Info.plist` (Infomation property list) 文件中声明你的 APP 支持 CoreBluetooth 后台运行模式。当你声明后，系统会唤醒 suspended 状态中的 APP 来处理蓝牙事件。这对于和每隔一段时间就传递数据的 BLE 设备进行交互的 APP 来说很重要，比如一个心率监测器。

APP 可能会声明的 CoreBluetooth 后台运行模式有两种。一种是 APP 扮演了 Central 的角色，另一种则是 APP 扮演了 Peripheral 的角色。如果你的 APP 同时扮演了这两种角色，你可以同时声明这两种后台运行模式。声明 CoreBluetooth 后台运行模式的方式就是在 `Info.plist` 文件中加入一个 `UIBackgroundModes` 的 Key，Value 则是包含以下提到的两种字符串的数组：

* `bluetooth-central` -- APP 使用 CoreBluetooth 框架与其他 BLE 设备进行通信。
*  `bluetooth-peripheral` -- APP 使用 CoreBluetooth 框架来分享数据。

提示：*Xcode 的属性列表编辑器默认显示的 Key 是人类易读的字符，而不是实际的 Key 的名字，要在 Info.plist 文件中显示实际 Key 名，按住 Control键并单击编辑器窗口中的任意 Key，并在弹出的上下文窗口中启用 `Show Raw Keys/Values` 项。比如 `UIBackgroundModes` 的易读 Key 名则是 `Required background modes`,如图所示*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/keyname.png" width="710" height="555" alt="选项列表图"/>
</div>


关于如何配置 `Info.plist` 文件内容的更多信息请查阅 [Xcode help](http://help.apple.com/xcode/mac)

#### bluetooth-central 后台运行模式

当扮演一个 Central 角色的 APP 在 Info.plist 文件中包含了 `UIBackgroundModes` `bluetooth-central` 键值对时，CoreBluetooth 框架允许你的 APP 在后台运行时执行蓝牙任务。当你的 APP 在后台运行时，你仍旧可以搜索和连接 Peripheral，然后与之进行数据交互。此外，当 `CBCentralManagerDelegate` 或者 `CBPeripheralDelegate` 的代理方法进行回调时，系统会唤醒你的 APP 允许你来处理这些 Central 端的重要事件。比如，成功建立了连接，连接中断，Peripheral 发送了一个更新值的通知，或者 Central Manager 的状态发生改变。

尽管你的 APP 在后台运行时可以执行许多蓝牙任务，但是你要注意这些，在后台运行状态下，当你搜索 Peripheral 的操作是不同于前台运行状态的。特别是当你 APP 在后台运行状态下搜索设备：

* 搜索参数 `CBCentralManagerScanOptionAllowDuplicatesKey`将会被忽略，多个广播数据的 Peripheral 发现事件将合并成一个发现事件。
* 如果所有的都在后台运行中搜索 Peripheral，则 Central 搜索广播数据包的时间间隔将会增加，你发现一个广播数据的 Peripheral 将会需要很长时间。

这种策略对降低 Radio 的使用频率和提高 iOS 设备的续航时间有帮助。


#### bluetooth-peripheral 后台运行模式

Peripheral 想要在后台执行蓝牙任务，你必须在 Info.plist 文件中包含了 `UIBackgroundModes` `bluetooth-peripheral` 键值对，这样系统会唤醒你的 APP 来处理读、写和订阅事件。

除了允许你的 APP 唤醒来处理读、写和来自 Central 的订阅请求外，CoreBluetooth 框架还允许你的 APP 在后台运行状态下广播数据。也就是说，你应该注意到，在后台状态下广播数据与在前台状态下的操作是不同的。特别是后台状态下广播数据：

* 广播的 `CBAdvertisementDataLocalNameKey ` Key 将被忽略，Peripheral 的 local name 也不会广播。
* 广播中 `CBAdvertisementDataServiceUUIDsKey` 的值的所有服务 UUID 都被放置在 “overflow” 区域中。只有当 iOS 设备进行显式搜索才会被发现。
* 如果所有 APP 都在后台运行状态下广播数据，你的 Peripheral 广播数据包的频率将会降低。

### 谨慎使用后台运行模式

对于一般用户情况，可能不需要声明 APP 支持其中一个或两个 CoreBluetooth 后台运行模式，你应该对后台进程负责，因为执行蓝牙的任务需要使用用户设备的 onboard radio（板载广播），使用广播会影响用户设备的续航时间，应尽量减少在后台执行蓝牙任务。APP 被蓝牙时间唤醒后应迅速处理该事件然后尽可能快速的进入 suspended 状态。

声明支持 CoreBluetooth 后台运行模式的 APP 必须遵守一些基本准则：

* APP 应该提供一个基于会话的界面来允许用户决定什么时候开始和结束发送蓝牙事件。
* 一个 APP 被唤醒时有 10秒 来完成一个任务，理想地，APP 应该尽快的完成任务然后再次进入 suspended 状态。APP 在后台运行时间太长会被系统限制或杀死。
* APP 不应该一直处于唤醒状态来处理与系统唤醒无关的额外任务。

在后台运行状态下，你的 APP 应该如何操作的详情请查阅 [Being a Responsible Background App](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html#//apple_ref/doc/uid/TP40007072-CH4-SW8).

### 在后台执行长期操作

一些 APP 可能需要使用 CoreBluetooth 框架来在后台运行状态下执行长期操作，比如，你想要为 iOS 设备开发一个 安全家庭 APP 来与配备 BLE 技术的门锁进行通信。APP 与门锁交互来自动锁门（当用户离开家的时候）和开锁（当用户回家的时候），整个期间 APP 一直在后台运行状态下。当用户离家的时候，iOS 设备终会超出门锁的有效范围，导致与门锁的连接中断。在这种情况下，APP 可以通过调用 `CBCentralManager ` 类的 `connectPeripheral:options: `该方法来建立连接，因为连接请求不会超时，这样当用户到家的时候就会与门锁重新连接。

现在试想一种情况，当用户离家有一些天了，如果此时 APP 被系统给终止了。这样当用户回家的时候，APP 将不能与门锁重新连接，用户也不能打开门锁了。类似这样的 APP，使用 CoreBluetooth 来执行长期操作至关重要，例如监视活动和挂起连接。

#### 状态保存和恢复

因为状态保存和恢复已经内置于 CoreBluetooth，你的 APP 可以选择加入这个功能来要求系统保存 APP 的 Central Manager 和 Peripheral Manager 的状态，并继续代表他们执行蓝牙任务，即使你的 APP 不在运行，当这些任务完成后，系统会重新让你的 APP 进入后台运行状态并给予一定的时间来保存状态和处理一些蓝牙事件。对于上述描述的安全家庭 APP 这种情况来说，系统将会监听连接请求，当用户回家的时候，系统将会重新启动 APP 来处理 `centralManager:didConnectPeripheral: ` 代理回调和完成连接请求。

CoreBluetooth 对 扮演 Central 角色，Peripheral 角色，或者同时都扮演的 APP 都提供状态保存和恢复支持。当扮演 Central 角色的 APP 增加状态保存和恢复时，当系统释放内存空间需要终止你的 APP 时，会保存你的 Central Manager 的状态，如果你的 APP 拥有多个 Central Manager，你可以选择你想要系统跟踪的 Central Manager，尤其，对于给定的 `CBCentralManager` 对象，系统会跟踪：

* Central Manger 搜索的 Service（和一些一开始指定搜索参数选项的）
* Central Manager 尝试连接或者已经连接的 Peripheral
* Central Manager 订阅的 Characteristic

扮演 Peripheral 角色的 APP 同样可以使用状态保存与恢复。对于 `CBPeripheralManager` 对象,系统会跟踪：

* Peripheral Manager 广播的数据
* Peripheral Manager 发布到设备库中的 Services 和 Characteristics
* 被 Central 订阅的 Characteristic 的值

当你的 APP 被系统置入后台运行状态时，你可以重新实例化 APP 的 Central Manager 和 Peripheral Manager 并恢复他们的状态。下面的章节会详细的描述如何在你的 APP 中使用状态保存和恢复。


#### 添加状态保存和恢复支持

CoreBluetooth 的状态保存和恢复是选择性加入的功能，需要以下步骤来让 APP 这一功能生效，你可以参考下面的步骤来给你的 APP 增加状态保存和恢复功能：

* 1、（*必须的*）当你 allocate 和 initialize 一个 Central Manager 或 Peripheral Manager 对象时需要加入状态保存和恢复。这一步在 `选择加入状态与恢复` 小节有详细描述。
* 2、（*必须的*）当系统启动 APP 的时候需要重新实例化一些 Central Manager 或 Peripheral Manager 对象。这一步在 `重新实例化你的 Central Manager 和 Peripheral Manager` 小节有详细描述。  
* 3、（*必须的*） 实现恰当的代理方法。这一步在 `实现恰当的 Restoration Delegate 方法` 小节有详细描述。
* 4、（*可选的*）更新你的 Central Manager 或 Peripheral Manager 的初始化进程。这一步在 `更新你的初始化进程` 小节中有详细描述。

##### 选择加入状态保存与恢复

选择加入状态保存和恢复功能后，当你 *allocate* 和 *initialize* 一个 Central Manager 或者 Peripheral Manager 的时候，你需要提供一个唯一的恢复标识符。恢复标识符是一个标识 Central Manager 或者 Peripheral Manager 的字符串，字符串的值对你的代码很重要，它将会告诉 CoreBluetooth 需要保存使用该标记的对象，CoreBluetooth 只保存这些拥有恢复标识符的对象。

举个例子，为你的 APP （只有一个 CBCentralManager 对象的 Central ）选择加入状态保存和恢复，当你初始化 Central Manager 的时候需要指定 options（初始化选项），选项是一个以 `CBCentralManagerOptionRestoreIdentifierKey` 为 key，Value 是 恢复标识符 的字典。示例代码如下：


```objc
myCentralManager =
    [[CBCentralManager alloc] initWithDelegate:self queue:nil
     options:@{ CBCentralManagerOptionRestoreIdentifierKey: @"myCentralManagerIdentifier" }];
``` 


虽然上述例子没有证明这一点，当你给使用 Peripheral Manager 对象的 APP 选择加入状态保存与恢复时应使用类似的方式：在你初始化 Peripheral Manager 的时候指定 options（初始化选项），参数是一个以 `CBCentralManagerOptionRestoreIdentifierKey` 为 key，以 *恢复标识符*字符串为 value 的字典。

提示：*因为一个 APP 可能拥有多个 CBCentralManager 和 CBPeripheralManager 对象的实例，你应该确保他们的恢复标识符是唯一的，这样系统就能正确地区分它们。*

##### 重新实例化你的 Central Manager 和 Peripheral Manager

当你的 APP 被系统从后台启动时，你需要做的第一件事就是用状态恢复标识符重新实例化合适的 Central Manager 和 Peripheral Manager,恢复标识符要跟它们第一次被创建的一样。如果你的 APP 仅仅用到了一个 Central Manager 或者 Peripheral Manager，并且该 Manager 在你的 APP 生命周期中还存活着，则你就不需要做这一步。

如果你的 APP 使用超过一个以上的 Central Manager 或者 Peripheral Manager，或者使用的 Manager 在 APP 的生命周期中已经死亡了。当你的 APP 被系统启动时，它需要知道要恢复哪一个 Manager 。你可以通过 APP 终止时保存的 Manager 对象的恢复标识符列表，使用合适的 launch option（启动选项）参数（`UIApplicationLaunchOptionsBluetoothCentralsKey` 或者 ` UIApplicationLaunchOptionsBluetoothPeripheralsKey`）在 `application:didFinishLaunchingWithOptions:` 代理方法回调中来获取恢复标识符数组。

举个例子，当系统启动 APP 时，你可以恢复所有 Central Manager 的恢复标识符，示例代码如下：

```objc
- (BOOL)application:(UIApplication *)application
didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
 
    NSArray *centralManagerIdentifiers =
        launchOptions[UIApplicationLaunchOptionsBluetoothCentralsKey];
    // ...
}
```


当你获得恢复标识符列表后，遍历这个数组来重新实例化合适的 Central Manager 对象。

提示：*当 APP 启动时，系统只会提供要执行一些蓝牙任务（此时 APP 没有在运行）的 Central Manager 和 Peripheral Manager 的恢复标识符。关于启动选项参数 Keys 的详情可以参考 [ UIApplicationDelegate Protocol Reference.
](https://developer.apple.com/reference/uikit/uiapplicationdelegate).*

##### 实现恰当的 Restoration Delegate 方法

当你恢复合适的 Central Manager 和 Peripheral Manager 后，然后将他们的状态与蓝牙系统的状态同步。为了使你的 APP 达到系统处理的速度，你必须实现恰当的 Restoration Delegate 方法，对 Central Manager ，需要实现 `centralManager:willRestoreState:` 代理方法，而对于 Peripheral Manager,则需要实现 `peripheralManager:willRestoreState:` 代理方法。

重要提示：*对于选择添加 CoreBluetooth 的状态保存与恢复的 APP，当 APP 从后台启动来完成一些蓝牙任务时，会第一个调用 `centralManager:willRestoreState:` 和 `peripheralManager:willRestoreState:` 代理方法，对于没有加入状态与恢复的 APP，会第一个调用 `centralManagerDidUpdateState:` 和 `peripheralManagerDidUpdateState:` 代理方法。*

以上提到的这些代理方法，最后一个参数是一个字典，该字典包含 APP 终止时系统保存的 Manager 的信息。字典中可用的 Keys，可以查阅 [Central Manager State Restoration Options](https://developer.apple.com/reference/corebluetooth/cbcentralmanagerdelegate) 和 [Peripheral_Manager_State_Restoration_Options](https://developer.apple.com/reference/corebluetooth/cbperipheralmanagerdelegate) 常量。

用 `centralManager:willRestoreState: ` 代理方法提供的字典的 Key 来恢复 CBCentralManager 对象的状态。举个例子，如果你的 Central Manager 拥有激活的或者中断的连接（APP 终止时），系统会继续监视 APP 的行为，如下所示，你可以通过 `CBCentralManagerRestoredStatePeripheralsKey ` Key 去得到 Central Manager 连接的或者尝试连接的 Peripheral 列表（用 CBPeripheral 对象表示）。

```objc
- (void)centralManager:(CBCentralManager *)central
      willRestoreState:(NSDictionary *)state {
 
    NSArray *peripherals =
        state[CBCentralManagerRestoredStatePeripheralsKey];
    // ...
}
```

当你获得 Peripheral 列表后然后干什么取决于使用情况。举个例子，如果 APP 有一个 Central Manager 搜索的 Peripheral 列表，你可能想把恢复的 Peripheral 加入该列表中。此时确保要设置 Peripheral 的 Delegate 以保证能收到合适的代理回调。

你可以通过类似的方法利用 `peripheralManager:willRestoreState: ` 代理方法提供的字典的 keys 来恢复 CBPeripheralManager 的状态。

##### 更新你的初始化进程

当你实现前面必须的步骤后，你可能想要查看 Central Manager 和 Peripheral Manager 的初始化进程。尽管这是一个可选的步骤，但对确保 APP 稳定运行很重要。举个例子，当正与连接的 Peripheral 数据传输到一半时，APP 终止了。当你的 APP 恢复这个 Peripheral 时，你不知道 APP 终止时数据处理到哪一步，你想要确定从哪里继续开始处理。

举个例子，当在 `centralManagerDidUpdateState: ` 代理方法中初始化 APP 时，如果你能够成功的从一个恢复的 Peripheral中找出指定的 Service（在 APP 终止之前），就像这样

```objc
NSUInteger serviceUUIDIndex =
    [peripheral.services indexOfObjectPassingTest:^BOOL(CBService *obj,
    NSUInteger index, BOOL *stop) {
        return [obj.UUID isEqual:myServiceUUIDString];
    }];
 
if (serviceUUIDIndex == NSNotFound) {
    [peripheral discoverServices:@[myServiceUUIDString]];
   // ...
}
```

以上示例中，如果系统在你调用 `discoverServices:` 方法完成 Service 搜索之前终止。如果 APP 搜索 Service 成功，你可以稍后查看是否发现合适的 Characteristic（一直订阅的） ，用这种方式来更新初始化进程，你需要在正确的时间调用正确的方法。

### 参考文献

1、[Core Bluetooth Background Processing for iOS Apps](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/CoreBluetoothBackgroundProcessingForIOSApps/PerformingTasksWhileYourAppIsInTheBackground.html#//apple_ref/doc/uid/TP40013257-CH7-SW1)

### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*