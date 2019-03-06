---
title: iOS BLE 开发小记[7] - 与小米手环 2 进行数据交互
date: 2017-05-08 15:45:00
categories: blog
tags: [BLE]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/05/08/iOS-Bluetooth-Low-Energy-Develop-Chapter7](http://muhlenxi.com/2017/05/08/iOS-Bluetooth-Low-Energy-Develop-Chapter7)*

### 导语：

> 根据前面学到的知识，在这一篇将以小米手环2为硬件，对前面的知识点进行实践操作。实践是检验真理的唯一标准！

<!-- more -->

关于 BLE 开发，有一款很要用的 APP 可以协助开发，它就是 [LightBlue](https://itunes.apple.com/us/app/lightblue-explorer-bluetooth-low-energy/id557428110?mt=8) ,有需要的小伙伴可以去 App Store 下载。

本文的 Demo 可以在 GitHub 下载，欢迎一起交流和学习！[传送门在这里](https://github.com/muhlenXi/Millet)

### 开发环境

* *Xcode：8.3.1*
* *Language： Swift 3.0*
* *Device：MI Band 2*

### 开始实践

*由于小米手环有配对机制，其他的 APP 连接后，过段时间，小米手环就会断开连接，解决这个问题的办法是先用小米手环官方的 APP 小米运动 配对后，然后退出。再开始本篇的文章的实践之路。*

#### 恢复配对数组

我们通过 `retrievePeripherals` 方法传入该手环的 UUID 来恢复系统过去曾经配对的 Peripheral 数组，如果数组不为空，则连接这个 Peripheral，示例代码如下：

```swift
// retrieve Peripheral
let uuid = UUID(uuidString: "32CDD66A-659D-8449-3F9A-ACCF201C8660")
let peripheralArray = self.centralManager.retrievePeripherals(withIdentifiers: [uuid!])
if peripheralArray.count > 0 {
    
    self.peripheral = peripheralArray.first!
    self.peripheral?.delegate = self
    // connecting peripheral
    self.centralManager .connect(self.peripheral!, options: nil)
    
} else {
    // begin discover peripherals
    self.centralManager.scanForPeripherals(withServices: nil, options: nil)
}
```

#### Discover Services

连接成功后，我们调用 `discoverServices` 方法来查找 Peripheral 的 Services，通过遍历 peripheral.services 数组打印，总共有以下几个 Service：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/service.png" width="660" height="300" alt="选项列表图"/>
</div>

### Discover Characteristic

接下来，我们分别调用 `discoverCharacteristics` 方法来分别查找每个 Service 的 Characteristic。通过遍历 service.characteristics 的数组，我们可以获取到每个 service 的 Characteristic。

通过查看 CBCharacteristicProperties 的声明信息，可以了解到，这是一个 struct。我们需要获取到每个属性的 rawValue，以便与下面对 Characteristic 的属性进行分析,通过打印，我们获取到的 rawValue 分别为，转为十六进制分别为：


| 属性        | rawValue    | 16进制0x表示  |
| ------------- |:-------------:| -----:|
| broadcast | 1 | 0x 0000 0001 |
| read | 2  | 0x 0000 0010 |
| writeWithoutResponse | 4  | 0x 0000 0100 |
| write | 8 | 0x 0000 1000 |
| notify  | 16 | 0x 0001 0000 |
| indicate | 32  |0x 0010 0000|
| authenticatedSignedWrites | 64 | 0x 0100 0000|
| extendedProperties | 128 | 0x 1000 0000 |


##### *180A 服务的特征：*

该服务共有 5 个特征，uuidString 分别为 2A25、2A27、2A28、2A23、2A50。该服务的特征大部分是一个硬件设备的信息。

*2A25：设备序列号* 

	<CBCharacteristic: 0x1740a3b40, UUID = Serial Number String, properties = 0x2, value = <62356663 36626433 61623464>, notifying = NO>
	
*2A27：固件版本号*

	<CBCharacteristic: 0x1700a5ac0, UUID = Hardware Revision String, properties = 0x2, value = <56302e31 2e332e32>, notifying = NO>
	
*2A28：固件版本号*

	<CBCharacteristic: 0x1700a5b20, UUID = Software Revision String, properties = 0x2, value = <56312e30 2e312e34 37>, notifying = NO>
	
*2A23：系统 ID*

	<CBCharacteristic: 0x1700a5b80, UUID = System ID, properties = 0x2, value = <d36c4aff fe4fb38f>, notifying = NO>
	
*2A50：PnP ID*

	<CBCharacteristic: 0x1740a3c60, UUID = PnP ID, properties = 0x2, value = <01570104 000001>, notifying = NO>

##### *00001530-0000-3512-2118-0009AF100700 服务的特征：*

该服务一共有 2 个特征，uuidstring 分别为 00001531-0000-3512-2118-0009AF100700 、00001532-0000-3512-2118-0009AF100700。

*00001531-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1700a5c40, UUID = 00001531-0000-3512-2118-0009AF100700, properties = 0x18, value = (null), notifying = NO>

*00001532-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1700a5ca0, UUID = 00001532-0000-3512-2118-0009AF100700, properties = 0x4, value = (null), notifying = NO>
	
##### *1811 服务的特征：*

该服务一共有 2 个特征，uuidstring 分别为 2A46 、2A44。

*2A46：*

	<CBCharacteristic: 0x1700a5d00, UUID = 2A46, properties = 0x8, value = (null), notifying = NO>

*2A44：*
	
	<CBCharacteristic: 0x1740a3120, UUID = 2A44, properties = 0x1A, value = <01>, notifying = NO>

##### *1802 服务的特征：*

*2A06：*

	<CBCharacteristic: 0x1740a4b00, UUID = 2A06, properties = 0x4, value = (null), notifying = NO>
	
##### *180D 服务的特征：*

*2A37：*

	<CBCharacteristic: 0x1740a4860, UUID = 2A37, properties = 0x10, value = (null), notifying = NO>

*2A39：*

	<CBCharacteristic: 0x1740a4bc0, UUID = 2A39, properties = 0xA, value = (null), notifying = NO>

##### *FEE0 服务的特征：*

该服务一共有 11 个特征，uuidstring 分别为2A2B、00000001-0000-3512-2118-0009AF100700、00000002-0000-3512-2118-0009AF100700、00000003-0000-3512-2118-0009AF100700、2A04、00000004-0000-3512-2118-0009AF100700、00000005-0000-3512-2118-0009AF100700、00000006-0000-3512-2118-0009AF100700、00000007-0000-3512-2118-0009AF100700、00000008-0000-3512-2118-0009AF100700、00000010-0000-3512-2118-0009AF100700

*2A2B：当前时间*

	<CBCharacteristic: 0x1740a4aa0, UUID = Current Time, properties = 0x1A, value = <e1070508 0e341801 000020>, notifying = NO>
	
*00000001-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1700a5d60, UUID = 00000001-0000-3512-2118-0009AF100700, properties = 0x14, value = (null), notifying = NO>

*00000002-0000-3512-2118-0009AF100700：*	

	<CBCharacteristic: 0x1740a47a0, UUID = 00000002-0000-3512-2118-0009AF100700, properties = 0x10, value = (null), notifying = NO>

*00000003-0000-3512-2118-0009AF100700：*	

	<CBCharacteristic: 0x1740a4560, UUID = 00000003-0000-3512-2118-0009AF100700, properties = 0x14, value = <10060700 01>, notifying = NO>

*2A04：周边首选连接参数*	

	<CBCharacteristic: 0x1740a4680, UUID = Peripheral Preferred Connection Parameters, properties = 0x16, value = <27002700 0000f401>, notifying = NO>
	
*00000004-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1740a46e0, UUID = 00000004-0000-3512-2118-0009AF100700, properties = 0x14, value = (null), notifying = NO>
	
*00000005-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1740a4500, UUID = 00000005-0000-3512-2118-0009AF100700, properties = 0x10, value = (null), notifying = NO>
	
*00000006-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1740a40e0, UUID = 00000006-0000-3512-2118-0009AF100700, properties = 0x12, value = <0f4f00e1 07041b02 050720e1 07041b06 320a2064>, notifying = NO>
	
*00000007-0000-3512-2118-0009AF100700：*	

	<CBCharacteristic: 0x1700a5dc0, UUID = 00000007-0000-3512-2118-0009AF100700, properties = 0x12, value = <0ccc0000 008a0000 00040000 00>, notifying = NO>

*00000008-0000-3512-2118-0009AF100700：*	

	<CBCharacteristic: 0x1700a5e20, UUID = 00000008-0000-3512-2118-0009AF100700, properties = 0x18, value = (null), notifying = NO>
	
*00000010-0000-3512-2118-0009AF100700：*	

	<CBCharacteristic: 0x1700a5e80, UUID = 00000010-0000-3512-2118-0009AF100700, properties = 0x10, value = (null), notifying = NO>
	


##### *FFE1 服务的特征：*

该服务一共有 9 个特征，uuidstring 分别为 00000009-0000-3512-2118-0009AF100700、FEDD、FEDE、FEDF、FED0、FED1、FED2、FED3、0000FEC1-0000-3512-2118-0009AF100700。

*00000009-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1740a45c0, UUID = 00000009-0000-3512-2118-0009AF100700, properties = 0x16, value = <100301>, notifying = NO>

*FEDD：*

	<CBCharacteristic: 0x1740a4260, UUID = FEDD, properties = 0x8, value = (null), notifying = NO>

*FEDE：*

	<CBCharacteristic: 0x1740a4020, UUID = FEDE, properties = 0x2, value = (null), notifying = NO>

*FEDF：*

	<CBCharacteristic: 0x1700a5be0, UUID = FEDF, properties = 0x2, value = (null), notifying = NO>

*FED0：*

	<CBCharacteristic: 0x1740a4200, UUID = FED0, properties = 0xA, value = (null), notifying = NO>

*FED1：*

	<CBCharacteristic: 0x1740a4d40, UUID = FED1, properties = 0xA, value = (null), notifying = NO>

*FED2：*

	<CBCharacteristic: 0x1740a4da0, UUID = FED2, properties = 0x2, value = (null), notifying = NO>

*FED3：*

	<CBCharacteristic: 0x1740a4e00, UUID = FED3, properties = 0xA, value = (null), notifying = NO>

*0000FEC1-0000-3512-2118-0009AF100700：*

	<CBCharacteristic: 0x1740a4e60, UUID = 0000FEC1-0000-3512-2118-0009AF100700, properties = 0x1A, value = (null), notifying = NO>



### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*
