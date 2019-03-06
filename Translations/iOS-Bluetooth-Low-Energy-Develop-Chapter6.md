---
title: iOS BLE 开发小记[6] - 设置本地设备为 Peripheral 的最佳实践
date: 2017-05-07 12:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/05/07/iOS-Bluetooth-Low-Energy-Develop-Chapter6](http://muhlenxi.com/2017/05/07/iOS-Bluetooth-Low-Energy-Develop-Chapter6)*

### 导语：

> 在这篇文章中，主要是将你的本地设备设置为一个广播数据的 Peripheral 的最佳实践，以及实际开发过程中应该注意的事项。

<!-- more -->

正如许多 Central 端的处理一样，CoreBluetooth 框架允许你控制 Peripheral 各方面的实现。这一篇将会给你提供一些准则和以负责的方式给你提供一些使用的最佳实践。

### 留意广播的数据

广播 Peripheral 的数据是实现设备本地设备为 Peripheral 的重要部分。接下来的小节将会帮助你以一些合理的方式来广播数据。

#### 遵守广播数据长度的限制

广播 Peripheral 的数据是通过调用 `CBPeripheralManager ` 类的 `startAdvertising:` 方法时传入广播数据的字典来完成的。当你创建一个广播数据字典时，需要注意你能广播数据的内容和长度限制。

尽管通常的广播数据包可以各种各样 Peripheral 设备的信息。也许你仅仅想广播设备的 Local Name
和你想要广播的服务的 UUID，也就是说，当你创建广播数据字典的时候，你仅仅能够指定以下两种 Key：`CBAdvertisementDataLocalNameKey` 和 `CBAdvertisementDataServiceUUIDsKey`,如果你指定其他的 Key，你将会收到错误提醒。

对广播数据的空间大小也有限制。当你的 APP 在前台运行状态时，你可以有 28 字节的空间用来初始化广播数据字典，该字典包含两个支持的 key。如果空间用尽，作为搜索响应将会额外增加 10 个字节的空间仅仅用于 Local name。任何服务的 UUID 不允许加入到这个专用的 “overflow” 空间。当用户设备显式搜索的时候才可以发现这个专用空间的内容。当你的 APP 在后台运行状态时，则不会广播 Local name，并且所有服务的 UUID 将会被放到这个特别空间里。

注意：*这些空间不包含每个新数据类型的 2 个字节的头部信息，广播数据和响应数据的正确格式在 Bluetooth 4。0 规范，第3卷，C部分，11章节有定义。*

为了在使空间大小在这些约束之内，仅允许广播主要的 Service UUID。

#### 仅仅广播你需要的数据

因为广播数据需要用到设备的 Radio ，会影响电池续航时间，因此当你想要别的设备连接你时，再广播数据。连接后，这些设备可以直接与 Peripheral 的数据进行交互，不需要其他广播数据包。因此，想要促进 BLE 的处理，应停止广播来最小化 Radio 的使用，进而增加 APP 的性能和保留设备的电量。通过调用 `CBPeripheralManager` 类的 `stopAdvertising` 方法即可。

```objc
[myPeripheralManager stopAdvertising];
```

##### 让用户来决定什么时候来广播

知道什么时候来广播通常用户比较清楚。举个例子，当你知道附近没有任何 BLE 设备时，在你的设备上 用 APP 广播服务是没有意义的。因为你的 APP 经常感知不到什么设备在附近，应在 APP 中提供一个 UI 界面来让用户选择何时广播。

### 配置你的 Characteristic

当你创建一个 mutable Characteristic，然后你设置它的属性，值，权限等。这些设定决定着 Central 如何连接和如何与 Characteristic 的值交互。虽然你可能基于你的 APP 的不同需要来配置 不同的Characteristic 属性、值、权限。当你需要执行以下的两种任务时，下面的小节将会提供一些指导：

* 允许 Central 连接来订阅 Characteristic
* 保护 Characteristic 的敏感信息不被未配对的 Central 访问

#### 配置你的 Characteristic 支持通知

在之前文章的描述中，我们知道对于经常改变值的 Characteristic，建议 Central 订阅该 Characteristic。 只要可能，应允许连接的 Central 来订阅你的 Characteristic 的值。

当你创建一个 mutable Characteristic 时，通过设置 Characteristic 的 properties 属性为 `CBCharacteristicPropertyNotify` 来支持订阅。就像这样：

```objc
myCharacteristic = [[CBMutableCharacteristic alloc]
        initWithType:myCharacteristicUUID
        properties:CBCharacteristicPropertyRead | CBCharacteristicPropertyNotify
        value:nil permissions:CBAttributePermissionsReadable];
```

示例中，Characteristic 的值是可读的，它可以被连接的 Central 订阅。

#### 需要一个配对的连接来访问敏感数据

根据使用情况，你可能想要发布一个服务，这个服务的一个或多个 Characteristic 的值是敏感的。举个例子，你可能想要发布一个 社交服务描述 服务，这个服务可能包含一些代表用户描述信息的 Characteristic，比如姓，名字，和 email 地址等。很有可能，你想要允许受信任的设备来获取用户的 email 地址。

你可以确保只有受信任的设备才能访问敏感 Characteristic 的值，你可以通过恰当的设置 Characteristic 的属性和权限来达到目的。继续上面提到的例子，只允许受信任的设备来获得用户的 email 地址，恰当的设置 Characteristic 的 properties 和 permissions，像这样：

```objc
emailCharacteristic = [[CBMutableCharacteristic alloc]
        initWithType:emailCharacteristicUUID
        properties:CBCharacteristicPropertyRead
        | CBCharacteristicPropertyNotifyEncryptionRequired
        value:nil permissions:CBAttributePermissionsReadEncryptionRequired];
```

示例代码中，Characteristic 配置的只允许受信任的设备来读和订阅它的值。当连接后，Remote Central 尝试读取或订阅这个 Characteristic 的值时， CoreBluetooth 将尝试将 Central 和本地设备配对来创建安全连接。

举个例子，如果 Central 和 Peripheral 都是 iOS 设备，双方都会弹出一个表示有设备想要配对的弹框。你需要将 Central 设备上弹框的配对码填入到 Peripheral 设备上弹框的文本框里来完成配对处理。

完成配对后，Peripheral 会认为该 Central 是个受信任的设备，并允许 Central 来访问它的加密Characteristic 的值。

### 参考文献

1、[Best Practices for Setting Up Your Local Device as a Peripheral](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/BestPracticesForSettingUpYourIOSDeviceAsAPeripheral/BestPracticesForSettingUpYourIOSDeviceAsAPeripheral.html#//apple_ref/doc/uid/TP40013257-CH5-SW1)

### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*
