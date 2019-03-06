---
title: iOS BLE 开发小记[5] - 与 Remote Peripheral 交互的最佳实践
date: 2017-05-05 11:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/05/05/iOS-Bluetooth-Low-Energy-Develop-Chapter5](http://muhlenxi.com/2017/05/05/iOS-Bluetooth-Low-Energy-Develop-Chapter5)*

### 导语：

> 在这一节，主要是与 Remote Peripheral 交互的最佳实践，以及实际开发过程中应该注意的事项。

<!-- more -->

CoreBluetooth 框架使 Central 端的工作变得容易透明、容易理解。也就是说，你的 APP 可以控制和负责 Central 的大部分方面，比如搜索设备、建立连接、与 Remote Peripheral 进行数据交互。本章将以负责的方式来提供一些规范和最佳实践，尤其是当你为 iOS 设备开发 APP 的时候。

### 要留意 Radio 的使用和电量的消耗

当开发一个 APP 与 BLE 设备交互时，需要铭记：BLE 通信会通过你的设备向空中发射 Radio 信号。其他形式的无线通信也可能需要使用设备的 Radio，比如，Wi-Fi，传统蓝牙以及使用 BLE 的其他 APP。因此要减少 Radio 的使用。

当开发 iOS 设备的 APP 时，最少次数使用 Radio 很重要，因为 Radio 的使用会对 iOS 设备电池的续航时间有不利影响。以下的这些规范将会帮助你更好的使用 Radio。作为回报，你的 APP 会良好运行和你设备电池的续航时间将会延长。

#### 只在需要的时候搜索周边设备

当你调用 `CBCentralManager` 类的 `scanForPeripheralsWithServices:options:` 方法来搜索正在广播数据的 Remote Peripheral 时，你设备的 Radio 会一直监听正在广播的设备，直到你停止搜索为止。

除非你需要搜索更多的设备，否则当你找到你想要连接的设备后就应该停止搜索，前几篇提到过，用 `CBCentralManager` 类的 `stopScan` 方法来停止搜索设备。

#### 必要的时候再指定 CBCentralManagerScanOptionAllowDuplicatesKey 选项

Remote Peripheral 设备可能会每秒发送多个广播数据包给监听的 Central，当你调用 `scanForPeripheralsWithServices:options: ` 方法搜索设备时，该方法默认的行为是将一个 Peripheral 的多个发现事件合并成一个发现事件，也就是说，Central Manager 每找到一个新的 Peripheral 时才会调用 `centralManager:didDiscoverPeripheral:advertisementData:RSSI: ` 代理方法，不管收到多少广播数据包都不会调用该方法。当已发现的 Peripheral 的广播数据发生变化时也会调用这一代理方法。

如果你想改变默认行为，调用 `scanForPeripheralsWithServices:options:` 方法时你可以指定 scan option 为 `CBCentralManagerScanOptionAllowDuplicatesKey`,这样每当 Central 收到来自 Peripheral 的数据包就会创建一个发现事件。对于某些情况关闭默认行为会很有用，比如基于 Peripheral 的 Proximity （靠近程度）来进行连接交互。Proximity 可以通过 Peripheral 的 接收信号强度指示（RSSI）的值来判断，也就是说，指定这个扫描选项会对电源的续航和 APP 的运行造成不利影响。因此，只在必要的情况下再指定该方法的 scan option。

#### 明智地获取 Peripheral 的数据

当你开发 APP 的时候，对于特定的使用情景，一个 Peripheral 设备可能拥的大量的 Service 和 Characteristic 已经超过了你需要的数量 ，搜索 Peripheral 全部的 Service 和 Characteristic 对电源的续航和 APP 的性能带来不利影响。因此。你应该只搜索你的 APP 需要的 Service 和 Characteristic。

举个例子，假如你连接的 Peripheral 有许多可用的 Service，但是你的 APP 只需要用到其中的两个。你只需要搜索这两个 Service 就可以了，只需要调用 `CBPeripheral` 类的 `discoverServices:` 方法时传入 Service UUID（用 CBUUID 对象表示） 数组就可以了。如下所示：

```objc
[peripheral discoverServices:@[firstServiceUUID, secondServiceUUID]];
```

当你搜索到这两个需要的 Service 后，你可以用相似的方式来搜索你需要的 Characteristic，同样地，在调用 `CBPeripheral` 类的 `discoverCharacteristics:forService: ` 方法时传入你想要的 Characteristic UUID（用 CBUUID 对象表示） 数组就可以了。

#### 订阅值频繁变化的 Characteristic

通过前面的文章，我们了解到，获取一个 Characteristic 值的方式有两种：

* 你可以在每次需要值的时候明确的调用 `readValueForCharacteristic:` 方法来读取 Characteristic 的值。
* 你可以调用 `setNotifyValue:forCharacteristic: ` 方法来订阅 Characteristic 的值，这样当值发生改变后就会收到来自 Peripheral 的通知。

对于值可能变化的 Characteristic 来说，订阅是最佳实践方式，尤其是对于值频繁改变的 Characteristic，关于如何订阅一个 Characteristic 的值，可以查阅 [Subscribing to a Characteristic’s Value](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/PerformingCommonCentralRoleTasks/PerformingCommonCentralRoleTasks.html#//apple_ref/doc/uid/TP40013257-CH3-SW16).

#### 当你得到你需要的数据后断开与设备的连接

当不再需要连接的时候与设备断开连接可以帮助你减少 Radio 的使用，你应该在以下的情景中断开与 Peripheral 设备的连接：

* 你所有订阅的 Characteristic 的值已经不再通知，你可以通过 Characteristic 的 `isNotifying` 属性来判断 Characteristic 的值是否通知。
* 你从 Peripheral 设备中得到了所有的数据。

在上述的场景中，取消一些订阅并与 Peripheral 断开连接，你可以通过调用 `setNotifyValue:forCharacteristic:` 方法来取消订阅一个 Characteristic 的值，设置第一个参数为 `NO` 。你可以通过调用 `CBCentralManager` 类的 `cancelPeripheralConnection:` 方法来断开与 Peripheral 设备的连接。像这样：

```objc
[myCentralManager cancelPeripheralConnection:peripheral];
```

提示：*`cancelPeripheralConnection:`方法是 nonblocking（非阻塞）的，一些 `CBPeripheral` 类的命令仍然等待 Peripheral，当你尝试断开连接可能没有完成执行.因为可能其他 APP 还在连接 Peripheral，取消一个本地连接不能保证设备物理连接立刻断开。从 APP 的角度来看，认为 Peripheral 是断开的，Central Manager 会调用 `centralManager:didDisconnectPeripheral:error: ` 代理方法进行回调。*

### 重新连接 Peripheral

使用 CoreBluetooth 框架，你有三种方式可以重新连接 Peripheral：

* 通过调用 `retrievePeripheralsWithIdentifiers:` 方法来恢复一个已知的 Peripheral 列表 --- Peripheral 是你发现的或者过去连接的。如果你要找的 Peripheral 在列表中，尝试去连接它。这种重连方式在 `恢复已知的 Peripheral 列表` 小节中有描述。

* 通过调用 `retrieveConnectedPeripheralsWithServices:` 方法来恢复系统当前已经连接的 Peripheral 列表，如果你要找的在列表中，则与它创建一个本地连接，这种重连方式在 `恢复当前已连接的 Peripheral 列表` 小节中有描述。

* 通过调用 `scanForPeripheralsWithServices:options:` 方法扫描和搜索 Peripheral，如果找到它，则连接。这种方式在 iOS BLE 开发小记[2]中有描述。

根据使用情况，每次重连 Peripheral ，你可能不想扫描和搜索同样的 Peripheral，你可能首先想用其他方式来重连。如图所示，一个可能的重连工作流程可能是按照上面的提到的顺序依次尝试这些方式。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/ReconnectingToAPeripheral_2x.png" width="519" height="510" alt="选项列表图"/>
</div>

提示：*你决定尝试的重连方式的个数取决于 APP 的使用情况，举个例子，你可能决定不使用第一种方式，或者你可能并行使用第一种和第二种方式。*

#### 恢复已知的 Peripheral 列表

当你第一次发现一个 Peripheral 时候，系统会创建一个标识符（用 NSUUID 对象表示的一个 UUID）来标识这个 Peripheral，你可以使用 NSUserDefaults 来保存这个标识符。过后你可以尝试调用`CBCentralManager` 类的 `retrievePeripheralsWithIdentifiers:` 方法来恢复并重新连接这个  Peripheral，下面描述的一种方式就是使用这个方法来重新连接以前的 Peripheral。

当 APP 启动时，调用 `retrievePeripheralsWithIdentifiers: ` 方法，传入一个包含 Peripheral 标识符的数组来发现和重连先前的 Peripheral，像这样：

```objc
knownPeripherals =
        [myCentralManager retrievePeripheralsWithIdentifiers:savedIdentifiers];
```

Central Manager 尝试在先前发现的 Peripheral中去匹配你提供的标识符，然后返回一个包含 CBPeripheral 对象的数组，如果没有匹配的对象，数组将会是空的。你应该去尝试其余的两种方式中的一种。如果数组不是空的，在界面中让用户去选择要重连哪一个。

当用户选择 Peripheral 后，调用 `CBCentralManager` 类的 `connectPeripheral:options:` 方法去连接 Peripheral。如果 Peripheral 设备被连接后， Central Manager 会调用 `centralManager:didConnectPeripheral: ` 代理方法，此时 Peripheral 重连成功。

提示：*Peripheral 可能因为一些原因不能连接，举个例子，设备不在 Central 附近，此外，一些 BLE 设备使用周期变化的随机设备地址。因此，即使设备在附近，从上次系统发现 Peripheral，设备的地址也可能发生改变，在这种情况下，你尝试连接的 CBPeripheral 对象与实际的 Peripheral 不相符。如果你不能重新连接 Peripheral，因为它的设备地址已经变了，你必须重新调用 `scanForPeripheralsWithServices:options:` 方法来搜索连接。*

关于设备随机地址的详细信息，请查阅 Bluetooth 4.0 规范，第3卷，C部分，10.8 章节和 [Bluetooth Accessory Design Guidelines for Apple Products](https://developer.apple.com/hardwaredrivers/BluetoothDesignGuidelines.pdf).

#### 恢复当前已连接的 Peripheral 列表

另一种重连 Peripheral 的方式就是检查你要连接的 Peripheral 是否一直被系统连接（举例，被另一个 APP 连接），你可以调用 `CBCentralManager` 类的 `retrieveConnectedPeripheralsWithServices:` 方法，它返回一个 CBPeripheral 对象的数组，数组中包含当前系统已经连接的 Peripheral。

因为系统当前可能连接超过一个以上的 Peripheral，你可以传入一个 CBUUID 对象的数组来恢复你指定 Service的 Peripheral，如果当前连接的 Peripheral 中没有你指定的 Peripheral，数组将是空的，你应该尝试其余两种方式中的一种。如果数组不为空，在界面中让用户去选择要重连哪一个。

假设用户选择了期望的 Peripheral，通过调用 `CBCentralManager` 类的 `connectPeripheral:options: ` 方法来本地化连接。即使系统一直连接着设备，你必须本地化连接后再进行交互。当建立了本地连接后，Central Manager 会调用 `centralManager:didConnectPeripheral: ` 代理方法，此时设备重连成功。


### 参考文献

1、[Best Practices for Interacting with a Remote Peripheral Device](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/BestPracticesForInteractingWithARemotePeripheralDevice/BestPracticesForInteractingWithARemotePeripheralDevice.html#//apple_ref/doc/uid/TP40013257-CH6-SW1)

### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*
