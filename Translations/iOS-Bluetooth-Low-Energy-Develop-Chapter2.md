---
title: iOS BLE 开发小记[2] - 如何实现一个 Local Central
date: 2017-04-29 10:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/04/29/iOS-Bluetooth-Low-Energy-Develop-Chapter2](http://muhlenxi.com/2017/04/29/iOS-Bluetooth-Low-Energy-Develop-Chapter2)*

### 导语：

> 在这一节，你将会学到，如何通过 CoreBluetooth 框架来实现 Local Central 方面的功能和代理方法。 

<!-- more -->

在 BLE 通信中，实现了 Central 规范的设备，能够调用许多常用的方法，比如搜索和连接可用的 Peripheral，然后与 Peripheral 提供的数据进行交互。但是实现了 Peripheral 规范的设备同样能够调用许多常见的方法，但是也有一些不同。比如，发布和广播 Service，对 Central 的读写请求进行响应以及 响应Central 的订阅请求。

在这个章节中，你将会学习在 Central 端如何使用 Core Bluetooth 框架来执行通用的 BLE 方法。基于代码的示例将会引导和协助你在你的本地设备上实现 Central 的角色。尤其，你将会学到：

* 如何创建一个 Central Manager 对象
* 如何搜索和连接一个正在广播信息的 Peripheral
* 连接成功后如何与 Peripheral 的数据进行交互
* 如何发送读取或写入请求给 Peripheral Service 的 Characteristic
* 如何订阅一个当数据更新时就会发出通知的 Characteristic

在下个章节，你将会学习如何在你的本地设备上实现 Peripheral 的角色。

你也许会发现本章节中的代码有点简单和抽象，在你的真实 App 中，你需要做些恰当的改变。更高级的开发技能可以参考后续的章节。


### Central 实现详情

**在本文中，你的 ViewController 需要遵循 `CBCentralManagerDelegate` 和 `CBPeripheralDelegate` 代理协议。**

#### 创建 Central Manager

因为在 CoreBluetooth 中是通过面向对象的思想用一个 CBCentralManager（中心管理） 对象来表示一个 Local Central 设备，所以在调用该对象的方法之前需要先 allocate（分配）和 initialize（初始化）一个 Central Manager 实例。可以通过 `initWithDelegate:queue:options: ` 方法来初始化一个 Central Manager 对象。 

```objc
myCentralManager =
        [[CBCentralManager alloc] initWithDelegate:self queue:nil options:nil];
```

在示例代码中，设置 Central Manager 的 Delegate 为 self，是为了接收 Central 的事件响应。 参数 dispatch queue（调度队列）设置为 nil，表示 Central Manager 是在 main queue（主队列）中分发响应事件。 

当你创建一个 Central Manager 对象时，Central Manager 会调用 `centralManagerDidUpdateState: ` 方法来代理回调。因此你必须实现这个代理方法来确保 Central 设备能够使用 BLE 技术,代理方法的详情见 [CBCentralManagerDelegate Protocol Reference](https://developer.apple.com/reference/corebluetooth/cbcentralmanagerdelegate)。

#### 搜索正在广播数据的 Peripheral 设备

初始化 Central Manger 对象后，第一个任务就是搜索周围的 Peripheral，上一篇曾提到过， Peripheral 通过广播数据的方式来显示它们的存在，你可以通过 `scanForPeripheralsWithServices:options:` 方法来搜索周围正在广播数据的 Peripheral 设备。

```objc
[myCentralManager scanForPeripheralsWithServices:nil options:nil];
```

关于参数说明：*如果第一个参数置为 nil 时，Central Manager 会返回所有正在广播数据的 Peripheral 设备。在实际 APP 开发中，通过指定一个包含 CBUUID 对象的数组来获取指定的 Peripheral，其中用一个 UUID（通用唯一识别码）来表示 Peripheral 正在广播的一个服务。关于 CBUUID 对象的详情见 [Services and Characteristics Are Identified by UUIDs](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/PerformingCommonPeripheralRoleTasks/PerformingCommonPeripheralRoleTasks.html#//apple_ref/doc/uid/TP40013257-CH4-SW8).*

每当 Central Manager 搜索到一个 Peripheral 设备时，就会通过 代理方法 `centralManager:didDiscoverPeripheral:advertisementData:RSSI:` 进行回调，新发现的 Peripheral 会以 CBPeripheral 对象的方式返回。如果你后面需要连接这个 Peripheral，需要用一个 CBPeripheral 类型的 Strong Reference（强引用）来指向这个对象，这样系统暂时就不会释放这个对象了。

```objc
- (void)centralManager:(CBCentralManager *)central
 didDiscoverPeripheral:(CBPeripheral *)peripheral
     advertisementData:(NSDictionary *)advertisementData
                  RSSI:(NSNumber *)RSSI {
 
    NSLog(@"Discovered %@", peripheral.name);
    self.discoveredPeripheral = peripheral;
    // ...
}
```
当你需要连接多个 Peripheral 设备时，需要使用一个 NSArray 来保存这些搜索到的 Peripheral，不管怎样，为了减少电量损耗，增加续航时间，只要搜索到你需要连接的 Peripheral 时，就可以停止搜索了。通过下面的方法可以停止搜索了。

```objc
[myCentralManager stopScan];
```

#### 连接刚发现的 Peripheral 设备

可以调用 `connectPeripheral:options: ` 方法来连接你想要连接的 Peripheral 设备。

```objc
[myCentralManager connectPeripheral:peripheral options:nil];
```

如果连接成功，Central 会通过 `centralManager:didConnectPeripheral:` 方法进行代理回调。在你与 Peripheral 进行交互时，你需要设置 Peripheral 的 Delegate 为 self 来确保能收到 Peripheral 的代理回调。

```objc
- (void)centralManager:(CBCentralManager *)central
  didConnectPeripheral:(CBPeripheral *)peripheral {
 
    NSLog(@"Peripheral connected");
    peripheral.delegate = self;
    // ...
}
```

#### 搜索刚连接 Peripheral 的 Service

当与 Peripheral 成功建立连接后，你就可以获取 Peripheral 的 Service 数据了，第一步就是搜索 Peripheral 提供的可用 Service。因为 Peripheral 对广播的数据包大小有限制，所以你可能会搜索到除了广播的 Service 之外的 其他 Service，你可以通过 `discoverServices: ` 方法搜索 Peripheral 提供的所有的 Service。

```objc
[peripheral discoverServices:nil];
```

提示：*在实际开发中，传入的参数一般不为 nil，传入 nil 会返回全部的可用 Service，为了节省电量以及一些不必要的时间浪费，通过指定一个 Service Array（包含 UUID 对象）为参数，来获取你想要了解的 Service 的信息，详情见 [Explore a Peripheral’s Data Wisely](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/BestPracticesForInteractingWithARemotePeripheralDevice/BestPracticesForInteractingWithARemotePeripheralDevice.html#//apple_ref/doc/uid/TP40013257-CH6-SW6).*

当搜索到指定的 Service 时，Peripheral 对象会通过 `peripheral:didDiscoverServices: ` 方法进行代理回调。CoreBluetooth 会生成一个数组用来保存 CBService 对象，你指定的 Service 就被包含在这个数组中。你可以通过实现下面这个代理方法来获取 Service 数组。

```objc
- (void)peripheral:(CBPeripheral *)peripheral
didDiscoverServices:(NSError *)error {
 
    for (CBService *service in peripheral.services) {
        NSLog(@"Discovered service %@", service);
        // ...
    }
    // ...
}
```

#### 搜索 Service 的 Characteristic

当你找到指定的 Service 之后，你下一步要做的就是搜索这个 Service 中提供的所有的 Characteristic，通过调用 `discoverCharacteristics:forService: ` 方法来搜索指定的 Service 的 Characteristic。

```objc
[peripheral discoverCharacteristics:nil forService:interestingService];
```

提示：*在实际开发中，第一个参数一般不传入 nil，因为你需要的 Characteristic 也许是所有 Characteristic 的一部分，为了节省电量以及一些不必要的时间浪费，通常指定一个 Characteristic Array（包含 UUID 对象）为参数，来获取你想要了解的 Characteristic 的信息。*

当搜索到指定的 Characteristic 后， Peripheral 会通过 ` peripheral:didDiscoverCharacteristicsForService:error: ` 方法进行代理回调，CoreBluetooth 会创建一个包含 CBCharacteristic 对象的数组用来保存指定的 Characteristic，如下是遍历数组中的 Characteristic。

```objc
- (void)peripheral:(CBPeripheral *)peripheral
didDiscoverCharacteristicsForService:(CBService *)service
             error:(NSError *)error {
 
    for (CBCharacteristic *characteristic in service.characteristics) {
        NSLog(@"Discovered characteristic %@", characteristic);
        // ...
    }
    // ...
}
```

#### 检索 Characteristic 的值

##### 读取某个 Characteristic 的值

当你找到 Service 指定的 Characteristic，可以通过 `readValueForCharacteristic:` 方法来读取这个 Characteristic 的值。

```objc
NSLog(@"Reading value for characteristic %@", interestingCharacteristic);
[peripheral readValueForCharacteristic:interestingCharacteristic];
```

当你尝试去读取一个 Characteristic 的值时， Peripheral 会通过 ` peripheral:didUpdateValueForCharacteristic:error: ` 代理回调来返回结果，你可以通过 Characteristic 的 value 属性来得到这个值。

```objc
- (void)peripheral:(CBPeripheral *)peripheral
didUpdateValueForCharacteristic:(CBCharacteristic *)characteristic
             error:(NSError *)error {
 
    NSData *data = characteristic.value;
    // parse the data as needed
    // ...
}
```

提示：*并不是所有的 Characteristic 的值都是可读的，决定一个 Characteristic 的值是否可读是通过检查 Characteristic 的 Properties 属性是否包含 CBCharacteristicPropertyRead 常量来判断的。当你尝试去读取一个值不可读的 Characteristic 时，Peripheral 会通过 `peripheral:didUpdateValueForCharacteristic:error: ` 给你返回一个合适的错误。*

##### 订阅一个 Characteristic 的值

通过 `readValueForCharacteristic: ` 方法读取一个 Characteristic 的静态值是有效的，但是，对于动态的值，就不是一个有效的方法，因为 Characteristic 的值在实时改变，比如你的心率数据。只有通过订阅才能获取实时的改变值，当你订阅一个 Characteristic 的值时，每当这个值发生改变时，你就会收到一个通知。

通过 `setNotifyValue:forCharacteristic:` 方法可以订阅指定 Characteristic 的值，该方法的第一个参数需要指定为 `YES`。

```objc
[peripheral setNotifyValue:YES forCharacteristic:interestingCharacteristic];
```

当你订阅或取消订阅一个 Characteristic 的值时，Peripheral 会通过调用 `peripheral:didUpdateNotificationStateForCharacteristic:error: ` 方法进行代理回调，如果订阅失败了，你可以通过这个方法获取到发生错误的原因。

```objc
- (void)peripheral:(CBPeripheral *)peripheral
didUpdateNotificationStateForCharacteristic:(CBCharacteristic *)characteristic
             error:(NSError *)error {
 
    if (error) {
        NSLog(@"Error changing notification state: %@",
           [error localizedDescription]);
    }
    // ...
}
```

提示：*并不是所有的 Characteristic 都提供订阅功能，决定一个 Characteristic 是否能订阅是通过检查 Characteristic 的 properties 属性是否包含 CBCharacteristicPropertyNotify 或者 CBCharacteristicPropertyIndicate  常量来判断的。*

##### 写数据到 Characteristic 中

有时需要写入一个数据到 Characteristic 中，比如，如果你的 APP 与数字恒温器进行交互。你或许想要给数字恒温器提供一个值，使得房间的室温能保持在这个温度左右。如果一个 Characteristic 的值是可写的，你可以通过调用 ` writeValue:forCharacteristic:type:` 方法将一个 data 类型（NSData 对象）的值写入到 Characteristic 中。

```objc
NSLog(@"Writing value for characteristic %@", interestingCharacteristic);
[peripheral writeValue:dataToWrite forCharacteristic:interestingCharacteristic
        type:CBCharacteristicWriteWithResponse];
```

当你写一个数据到 Characteristic 中时，你可以指定写入类型，上面代码中的写入类型是 `CBCharacteristicWriteWithResponse` ，此类型时，Peripheral 会通过 `peripheral:didWriteValueForCharacteristic:error: ` 方法来代理回调告知你是否写入数据成功，可以实现下面这个代理方法进行错误处理。

```objc
- (void)peripheral:(CBPeripheral *)peripheral
didWriteValueForCharacteristic:(CBCharacteristic *)characteristic
             error:(NSError *)error {
 
    if (error) {
        NSLog(@"Error writing characteristic value: %@",
            [error localizedDescription]);
    }
    // ...
}
```

如果你指定写入类型为 `CBCharacteristicWriteWithoutResponse` 时，不能保证写入操作是否有效的执行了。这时 Peripheral 不会调用任何代理方法，如果想了解 CoreBluetooth 提供的写入类型详情，可以查阅 [CBCharacteristicWriteType](https://developer.apple.com/reference/corebluetooth/cbcharacteristicwritetype).

提示：*有的 Characteristic 的值可能仅仅是可写的，或者不是可写的。决定 Characteristic 的值是否可写，需要通过查看 Characteristic 的 properties 属性是否包含 CBCharacteristicPropertyWriteWithoutResponse 或者 CBCharacteristicPropertyWrite 常量来判断的。*

### 参考文献

1、[Performing Common Central Role Tasks](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/PerformingCommonCentralRoleTasks/PerformingCommonCentralRoleTasks.html#//apple_ref/doc/uid/TP40013257-CH3-SW1)


### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*