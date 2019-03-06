---
title: iOS BLE 开发小记[3] - 如何实现一个 Local Peripheral
date: 2017-05-01 10:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/05/01/iOS-Bluetooth-Low-Energy-Develop-Chapter3](http://muhlenxi.com/2017/05/01/iOS-Bluetooth-Low-Energy-Develop-Chapter3)*

### 导语：

> 在这一节，你将会学到，如何通过 CoreBluetooth 框架来实现 Local Peripheral 方面的功能和代理方法。 

<!-- more -->

在 iOS BLE 开发小记[2]中，你已经学到了如何在 Central 方面去调用 BLE 的常用方法。在这一节中，你将学习用 CoreBluetooth 框架来调用 Peripheral 方面 BLE 的常用方法。通过本文的示例代码，将会引导你开发一个将你的 Local 设备实现为 Local Peripheral。你将会从中学到：

* 如何创建一个 Peripheral Manager 对象
* 如何为你的 Local Peripheral 设置 Services 和 Characteristics
* 如何发布你的 Services 和 Characteristics 数据
* 如何广播你的设备
* 如何对连接的 Central 做读写请求响应
* 如何发送更新后的值给订阅的 Central

或许你发现示例代码过于简单和抽象，你需要在你的 App 中做些恰当的练习来掌握这些内容。更高级的技巧和最佳实践在后续的文章中将会讲解。

### Peripheral 实现详情

#### 创建一个 Peripheral Manager 对象

在 Local Device（当前设备）实现 Peripheral 规范的第一步是分配（allocate）和初始化（initialize）一个周边管理（Peripheral Manager），（用 CBPeripheralManager 对象表示），通过调用 CBPeripheralManager 的 `initWithDelegate:queue:options: ` 方法来创建管理对象，如下所示

```objc
myPeripheralManager =
        [[CBPeripheralManager alloc] initWithDelegate:self queue:nil options:nil];
```

在示例代码中，设置 Delegate 为 self 是为了接收 Peripheral 的事件响应，将参数 dispatch queue 置为 nil。意味着 Peripheral Manager 将会在主队列中分发事件。

当你创建一个 Peripheral Manger 对象时，Peripheral Manager 会通过 `peripheralManagerDidUpdateState: ` 方法来代理回调，你必须实现这个代理方法来确保当前设备是否支持 BLE 技术，关于代理方法的详情可以查阅 [CBPeripheralManagerDelegate Protocol Reference](https://developer.apple.com/reference/corebluetooth/cbperipheralmanagerdelegate).

#### 设置你的 Services 和 Characteristics

在第一节中，我们了解到，一个 Local Peripheral 采用树形结构来组织 Services 和 Characteristics 的数据。因此必须采用树形结构方式来设置 Local Peripheral 的 Services 和 Characteristics。你第一步要做的是搞清和理解 Service 和 Characteristic 是如何标识的。

##### 通过 UUID 标识 Services 和 Characteristics

Peripheral 的 Service 和 Characteristic 是通过 128 位的特定蓝牙 UUID（通用唯一识别码）来标识的，在 CoreBluetooth 中是用 CBUUID 对象来表示的。并不是所有的 UUID 都是通过 Bluetooth Special Interest Group （蓝牙特别兴趣小组）预定义的。为了方便起见，Bluetooth SIG 定义和发布了许多通用的 16位 UUID。举个例子，Bluetooth SIG 事先定义了一个16位的 UUID 用来标识一个心率 Service，该 UUID 是 128位 UUID 0000180D-0000-1000-8000-00805F9B34FB 进行缩减而来的，这是基于蓝牙 4.0 规范中，第 3 卷 F 部分第 3.2.1 节定义的蓝牙基础 UUID。

CBUUID 提供了一个处理比较长的 UUID 的工厂方法，举个例子，生成一个表示心率 Service 的 UUID，可以调用 ` UUIDWithString` 方法来通过预定义的 16位 UUID来创建 CBUUID 对象。

```objc
CBUUID *heartRateServiceUUID = [CBUUID UUIDWithString: @"180D"];
```

当你通过预定义的 16位 UUID 来创建 CBUUID 对象时，CoreBluetooth 会基于128位Bluetooth Base UUID 填充剩下的的 UUID 位。

##### 为你定制的 Services 和 Characteristics 生成 UUID

你的 Service 和 Characteristic 的UUID也许可能没有被 Bluetooth UUIDs 预定义，如果没有被预定义，你需要手动生成你自己的 128位 UUID 来表示 Service 和 Characteristic。

通过命令行命令 `uuidgen` 可以生成 128位的 UUID，打开你的 Terminal（终端），通过这种方式依次为你的 Services 和 Characteristics 生成一个 UUID （用连字符连接起来的字符串）来标识。举例如下：

```objc
$ uuidgen
71DA3FD1-7E10-41C1-B16F-4430B506CDE7
```

你可以用上面方法生成的 UUID 调用 `UUIDWithString` 方法来创建一个 CBUUID 对象。

```objc
 CBUUID *myCustomServiceUUID =
        [CBUUID UUIDWithString:@"71DA3FD1-7E10-41C1-B16F-4430B506CDE7"];
```

##### 构建你的 服务特征树

当你为每个 Service 和 Characteristic 创建 CBUUID 对象后，你可以创建 mutable Service（可变服务） 和 mutable Characteristic（可变特征），然后以树形的方式组织它们。举个例子，如果你现在有一个 Characteristic 的 UUID，你可以通过调用 CBMutableCharacteristic 类的 `initWithType:properties:value:permissions:` 方法生成一个 mutable Characteristic。

```objc
myCharacteristic =
        [[CBMutableCharacteristic alloc] initWithType:myCharacteristicUUID
         properties:CBCharacteristicPropertyRead
         value:myValue permissions:CBAttributePermissionsReadable];
```

当你创建 mutable Characteristic 的时候，你可以指定它的 properties（属性）、value（值）和 permissions（权限许可），你指定的 properties 和 permissions 决定这个 Characteristic 的值是否可以读或者写，或者连接的 Central 能否订阅该 Characteristic 的值。下面的示例中，Characteristic 的值是被指定为可读的。关于 mutable Characteristic 的 properties 和 permissions 详情可以查阅 [CBMutableCharacteristic Class Reference](https://developer.apple.com/reference/corebluetooth/cbmutablecharacteristic).

提示：*如果你指定了 Characteristic 的值，那么该值将被缓存并且该 Characteristic 的 properties 和 permissions 将被设置为可读的。因此，如果你需要 Characteristic 的值是可写的，或者你希望在 Service 发布后，Characteristic 的值在 lifetime（生命周期）中依然可以更改，你必须将该 Characteristic 的值指定为 nil。通过这种方式可以确保 Characteristic 的值,在 Peripheral Manager 收到来自连接的 Central 的读或者写请求的时候，能够被动态处理。*

既然你创建了一个 mutable Characteristic，你也能通过调用 `CBMutableService` 类的 `initWithType:primary: ` 方法创建一个 mutable Service。如下所示：

```objc
myService = [[CBMutableService alloc] initWithType:myServiceUUID primary:YES];
```

在示例代码中，第二个参数被指定为 YES，用来表明该 Service 是 Primary（主要的），而不是 secondary（次要的）。一个 Primary Service 用来描述这个设备的主要功能，还可以用来引用其他的 Service。一个 Secondary Service 用来描述的是上下文中相关的或者被引用的 Service。举个例子，从心率传感器中获取心率的服务是 primary Service，而获取传感器电量的服务就可以被视为 secondary Service 。

当你创建完 Service 后。你需要设置 Service 的 Characteristic 数组属性，如下：

```objc
myService.characteristics = @[myCharacteristic];
```

#### 发送你的 Services 和 Characteristics

当你构建好服务特征树后，下一步就是按照 BLE 的规范发布到设备的服务特征库中，用 CoreBluetooth 可以很轻松的完成这一步，只需要调用 `CBPeripheralManager`类 的 `addService:` 方法就可以了。 示例代码如下：

```objc
[myPeripheralManager addService:myService];
```

当你调用该方法发布服务时，Peripheral Manager 会调用 ` peripheralManager:didAddService:error: ` 方法进行代理回调，实现这个代理方法可以获取到产生的错误，示例代码如下：

```objc
- (void)peripheralManager:(CBPeripheralManager *)peripheral
            didAddService:(CBService *)service
                    error:(NSError *)error {
 
    if (error) {
        NSLog(@"Error publishing service: %@", [error localizedDescription]);
    }
    // ...
}
```

提示：*当你发布 Service 和相关的 Characteristic 到 Peripheral 的数据库中后，设备已经将数据缓存，你不能再改变它了。*

#### 广播你的 Service

当你发布你的 Service 和 Characteristic 到设备的服务特征库时，你可以广播一些服务给正在监听的 Central，你可以通过调用 `CBPeripheralManager` 类的 `startAdvertising:` 方法来开始广播，传入的字典是要广播的数据。

```objc
[myPeripheralManager startAdvertising:@{ CBAdvertisementDataServiceUUIDsKey :
        @[myFirstService.UUID, mySecondService.UUID] }];
```

在示例代码中，传入的字典中唯一的 key 是 `CBAdvertisementDataServiceUUIDsKey`,用一个包含 CBUUID 对象的数组来表示你想要广播的服务的 UUID。你在字典中可以指定的其他 key 在 [Advertisement Data Retrieval Keys](https://developer.apple.com/reference/corebluetooth/cbcentralmanagerdelegate) 中有详细说明。也就是说，仅有 `CBAdvertisementDataLocalNameKey ` 和 `CBAdvertisementDataServiceUUIDsKey` 这两个 key 支持 Peripheral Manager 对象。

当你在本地设备中广播一些数据时，Peripheral Manager 会通过 `peripheralManagerDidStartAdvertising:error: ` 方法来代理回调。如果你的设备不能广播而发生错误时，实现这个代理方法可以获取产生的错误：

```objc
- (void)peripheralManagerDidStartAdvertising:(CBPeripheralManager *)peripheral
                                       error:(NSError *)error {
 
    if (error) {
        NSLog(@"Error advertising: %@", [error localizedDescription]);
    }
    // ...
}
```

提示：*广播数据方法会被尽力执行，因为空间是有限的和多个 APP 可能同时需要广播数据，更多详情可以查阅关于 [startAdvertising:](https://developer.apple.com/reference/corebluetooth/cbperipheralmanager/1393252-startadvertising) 方法的讨论。*

*当你的 APP 在后台运行时也会影响广播的行为，这一内容将会在下一篇中进行讨论。* 

#### 响应 Central 的读写请求

当你连接一个或多个 Central 后，你可能会收到读或者写的请求，对这些请求作出响应需要采取恰当的方式，下面的示例代码将会描述如何处理这些请求。

当一个连接的 Central 发送读取某个 Characteristic 数据的请求时，Peripheral Manager 会调用 `peripheralManager:didReceiveReadRequest: ` 方法进行代理回调。代理方法以 `CBATTRequest` 对象的方式来传递请求，它包含一些请求的属性。

比如，当你收到一个读取 Characteristic 值的简单请求时，可以通过代理方法回调的 `CBATTRequest` 对象来判断 Central 指定要读取的 Characteristic 是否和设备服务库中的 Characteristic 是否相匹配。你可以开始实现这个代理方法，示例代码如下：

```objc
- (void)peripheralManager:(CBPeripheralManager *)peripheral
    didReceiveReadRequest:(CBATTRequest *)request {
 
    if ([request.characteristic.UUID isEqual:myCharacteristic.UUID]) {
        // ...
    }
}
```

如果 Characteristic 的 UUID 能够匹配，下一步就是确保读取请求的位置没有超出 Characteristic 的值的边界。如下面代码所示，你可以通过使用 `CBATTRequest` 对象的 `offset` 属性来确保读取请求没有尝试读取范围之外的数据。

```objc
if (request.offset > myCharacteristic.value.length) {
    [myPeripheralManager respondToRequest:request
        withResult:CBATTErrorInvalidOffset];
    return;
}
```

假如读取请求的 offset（偏移）已经确认，现在就可以设置请求的 Characteristic 的属性（默认值为 nil）为你设备中的 Characteristic 的值了，你应该重视读取请求的偏移：

```objc
request.value = [myCharacteristic.value
        subdataWithRange:NSMakeRange(request.offset,
        myCharacteristic.value.length - request.offset)];
```

设置完值后，通过调用 `respondToRequest:withResult:` 方法并传入 request（更新值后的）和 请求的结果参数来对 Remote Central 的请求作出响应表示请求已经被成功处理。示例代码如下：

```objc
[myPeripheralManager respondToRequest:request withResult:CBATTErrorSuccess];
// ...
```

只要代理方法 `peripheralManager:didReceiveReadRequest:` 方法被回调，就需要准确的调用 `respondToRequest:withResult:` 方法。

提示：*如果 Characteristic 的 UUID 不匹配，或者因为某种原因不能完全读取，不必去填充请求，直接调用 `respondToRequest:withResult: ` 方法并提供一个表示失败的结果即可。你可能指定的结果列表见 [CBATTError Constants](https://developer.apple.com/reference/corebluetooth/cbatterror.code) 常量枚举。*

处理连接的 Central 写入请求也比较易懂。当 Central 发送一个写入请求给一个或多个你的 Characteristic 时，Peripheral Manager 会通过 ` peripheralManager:didReceiveWriteRequests: ` 方法来代理回调。这是，代理方法会传递一个包含一个或多个 `CBATTRequest` 对象的数组给你，数组中的每个对象都代表一个写入请求。当你确定写入请求能够处理时，你可以设置 Characteristic 的值，示例代码如下：

```objc
myCharacteristic.value = request.value;
```

虽然上述例子没有证明这一点，但当你给 Characteristic 写数据的时候，你应该确保请求的 offset 属性的范围有效。

就像你响应读取请求一样，只要代理方法 `peripheralManager:didReceiveWriteRequest:` 方法被回调，就需要准确无误的调用 `respondToRequest:withResult:` 方法。也就是说，`respondToRequest:withResult:` 方法期望有一个 `CBATTRequest` 对象，即使你可能通过 `peripheralManager:didReceiveWriteRequests:` 代理方法接收到一个包含 `CBATTRequest` 对象的数组，你也应该传入数组中的第一个对象，示例代码如下：

```objc
[myPeripheralManager respondToRequest:[requests objectAtIndex:0]
        withResult:CBATTErrorSuccess];
```

提示：*将多请求视为单一请求来对待，如果个别的请求不能被填充，你就不必填充其余的请求了，直接调用 `respondToRequest:withResult: ` 方法并提供一个表示失败的结果即可。*

#### 发送更新 Characteristic 的通知给订阅的 Central

连接的 Central 经常会订阅一个或多个 Characteristic 的值，当这些值发生变化时，你应该发送通知给订阅的 Central 。

当一个连接的 Central 订阅一个或多个你的 Characteristic 值时，Peripheral Manager 会通过 `peripheralManager:central:didSubscribeToCharacteristic:` 方法来代理回调。示例代码如下：

```objc
- (void)peripheralManager:(CBPeripheralManager *)peripheral
                  central:(CBCentral *)central
didSubscribeToCharacteristic:(CBCharacteristic *)characteristic {
 
    NSLog(@"Central subscribed to characteristic %@", characteristic);
    // ...
}

```

将上述的代理方法作为一个线索来开始给 Central 发送更新后的值。

接着，获取更新后的 Characteristic 的值，通过调用 `CBPeripheralManager`类的 `updateValue:forCharacteristic:onSubscribedCentrals:` 方法来给 Central 发送通知。示例代码如下：

```objc
NSData *updatedValue = // fetch the characteristic's new value
BOOL didSendValue = [myPeripheralManager updateValue:updatedValue
    forCharacteristic:characteristic onSubscribedCentrals:nil];
```

当你调用这个方法给订阅的 Central 发送通知时，你可以通过最后的那个参数来指定要发送的 Central，示例代码中的参数为 nil，表明将会发送通知给所有连接且订阅的 Central，没有订阅的 Central 则会被忽略。

`updateValue:forCharacteristic:onSubscribedCentrals: ` 方法会返回一个 Boolean 类型的值来表示通知是否成功的发送给订阅的 Central 了，如果 base queue （基础队列）满载，该方法会返回 NO，当传输队列存在更多空间时，Peripheral Manager 则会调用 `peripheralManagerIsReadyToUpdateSubscribers: ` 代理方法进行回调。你可以实现这个代理方法，在方法中再次调用 `updateValue:forCharacteristic:onSubscribedCentrals:` 方法发送通知给订阅的 Central。

提示：*用通知发送单个数据包给订阅的 Central，就是说，一旦订阅的 Central 发生更新时，你就应该调用 `updateValue:forCharacteristic:onSubscribedCentrals:` 方法用单一通知发送全部的更新值。*

*并不是所有的数据都是通过通知来传输的，这主要取决于你的 Characteristic 的值的大小，只有当 Central 调用 
`CBPeripheral`类的 `readValueForCharacteristic: ` 方法时，你可以检索全部的值。*

### 参考文献

1、[Performing Common Peripheral Role Tasks](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/PerformingCommonPeripheralRoleTasks/PerformingCommonPeripheralRoleTasks.html#//apple_ref/doc/uid/TP40013257-CH4-SW1)


### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*

