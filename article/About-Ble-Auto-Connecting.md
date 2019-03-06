---
title: 对 BLE 外设（Peripheral）自动重连机制的逻辑梳理
date: 2017-07-07 14:37:36
categories: blog
tags: [BLE,Auto Connecting]

---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: <http://muhlenxi.com/2017/07/07/About-Ble-Auto-Connecting>*

### 导语：

> 近期一直在做 BLE 蓝牙手环的一些项目，也积累了一些经验，为了不再被这些问题劳生伤神，还是记录下来比较靠谱。目前，CoreBluetooth 与有手环（Peripheral）两种连接方式，一种是连接后就可以直接通信了，另一种则是，连接后需要配对，配对后就可以通信了。本文会针对两种不同的机制的自动重连方式进行探索和记录。

<!-- more -->

到目前为止，感受最深的是，敲代码跟写作文有点类似，有思路的时候，行云流水，一气呵成。没思路的时候，就跟挤牙膏似得，还问题不断。为了提高以后行云流水的能力，就把一些比较绕的逻辑记录下来。

### 【1】直接连接通信（无需配对的情况）

这种情况，就是每次都得 Scan Peripheral ，找到你想要的 peripheral 后，然后进行 connect 操作，connect 成功后，就可以为所欲为了。

这种情况下，当 peripheral 因为一些原因断开连接或者用户主动断开连接导致连接中断，CoreBluetooth 会调用 `- (void)centralManager:(CBCentralManager *)central didDisconnectPeripheral:(CBPeripheral *)peripheral error:(NSError *)error` 代理方法，你若你想要自动重连的话，需要进行以下操作。

##### 步骤 1 

**创建一个 `断开原因` 标志位，用于标识断开的原因，用于判断断开原因是用户主动断开的？还是其他原因断开的？**

比如：

```objc
@property (nonatomic,copy) NSString * disConnectedState; //!< 1-其他原因断开，自动重连 3-手动断开，不重连
```

当然，这种情况 enum 是最合理的方式。

##### 步骤 2 

**创建一个 `连接方式` 标志位，用于标识连接的方式，用于判断连接方式是用户点击 Peripheral 列表连接的？还是自动重连以前连接过的 Peripheral ？**

比如：

```objc
@property (nonatomic,copy) NSString * connectedMethod;  //!<  1 - 自动重连, 2 - 点击连接
```

当然，这种情况 enum 是最合理的方式。

##### 步骤 3

**通过 `NSUserDefaults` 记录曾经连接过的 Peripheral 的名字，前提是这个名字可以唯一标识这个 peripheral。**

比如，在连接成功的代理方法中保存 peripheral 的名字。

```objc
// 保存外设名字
[[NSUserDefaults standardUserDefaults] setObject:peripheral.name forKey:@"PeripheralName"];
```

##### 步骤 4

**在 `- (void)centralManagerDidUpdateState:(CBCentralManager *)central` 代理方法中决定是否要调用 `60秒重连方法自动重连方法`，并且在自动重连方法中需要将连接方式标志位设为自动重连方式。**

```objc
- (void)centralManagerDidUpdateState:(CBCentralManager *)central
{
    switch (central.state) {
        case CBCentralManagerStateUnknown:
            NSLog(@">>> CBCentralManagerStateUnknown");
            break;
        case CBCentralManagerStateResetting:
            NSLog(@">>> CBCentralManagerStateResetting");
            break;
        case CBCentralManagerStateUnsupported:
            NSLog(@">>> CBCentralManagerStateUnsupported");
            break;
        case CBCentralManagerStateUnauthorized:
            NSLog(@">>> CBCentralManagerStateUnauthorized");
            break;
        case CBCentralManagerStatePoweredOff:
            NSLog(@">>> CBCentralManagerStatePoweredOff");
            break;
        case CBCentralManagerStatePoweredOn:
        {
            NSLog(@">>> CBCentralManagerStatePoweredOn");
            // 其他原因断开 并且 peripheral 的名字不为 nil
            NSString * existName = [[NSUserDefaults standardUserDefaults] objectForKey:@"PeripheralName"];
            if ([self.disConnectedState isEqualToString:@"1"] && existName != nil) {
                [self autoScanAndConnectedToExistPeripheralOnOneMinutes];
            }
        }
            break;
        default:
            break;
    }
}
```

*以下为60S自动重连方法*

```objc
// 自动去搜索之前连过的外设并尝试一分钟重连
- (void) autoScanAndConnectedToExistPeripheralOnOneMinutes
{
	// 如果之前连接的 peripheral 为 nil，则需要先搜索到这个 peripheral 后再连接，如果不为 nil ，则直接连接处理。
    if (self.peripheral == nil) {
         [self.centralManager scanForPeripheralsWithServices:nil options:nil];
    } else {
        [self.centralManager connectPeripheral:self.peripheral options:nil];
    }
    
    // 正在连接的加载动画
    [self.centralManager scanForPeripheralsWithServices:nil options:nil];
    [SVProgressHUD setDefaultMaskType:SVProgressHUDMaskTypeBlack];
    [SVProgressHUD setDefaultStyle:SVProgressHUDStyleCustom];
    [SVProgressHUD setBackgroundColor:[UIColor blackColor]];
    [SVProgressHUD setForegroundColor: [UIColor whiteColor]];
    [SVProgressHUD showWithStatus:NSLocalizedString(@"In the connection...", @"连接中")];
    
    // 连接方式为自动重连
    self.connectedMethod = @"1";
    
    // 60秒后超时处理
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(60 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        
        // 如果 60 S后还未找到连接过的 peripheral 则取消搜索和连接。
        if (self.peripheral == nil ) {
            
            // 停止搜索
            [self.centralManager stopScan];

            // 取消连接，为了避免死循环，该取消连接方式为主动断开连接
            self.disConnectedState = @"3";
            [self.centralManager cancelPeripheralConnection:self.peripheral];
            
            [SVProgressHUD dismiss];
            
        }    
    });
}

```

**如果是用户点击连接的话，需要在 Cell 的 didSelectRowAtIndexPath 代理方法中进行连接并设置连接方式标志。**

```objc
//点击方法
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 取消连接
    [[AppDelegate mainDelegate] cancelScanPeripherals];
    
    // 保存用户指定的 Peripheral
    CBPeripheral * per = [AppDelegate mainDelegate].bleArray[indexPath.row];
    [AppDelegate mainDelegate].peripheral = per;
    
    [SVProgressHUD showWithStatus:NSLocalizedString(@"Linking Bluetooth devices...", @"name")];
    
    // 设置连接方式为用户点击连接方式
    [AppDelegate mainDelegate].connectedMethod = @"2";
    
    // 连接外设
    [[AppDelegate mainDelegate] connectToPeripheralOnOneMinutes];
    
}
```


##### 步骤 5

**在 `- (void)centralManager:(CBCentralManager *)central didDiscoverPeripheral:(CBPeripheral *)peripheral advertisementData:(NSDictionary<NSString *, id> *)advertisementData RSSI:(NSNumber *)RSSI` 扫描外设代理方法中区别对待不同连接方式的Peripheral，如果是自动重连模式下的，发现 Peripheral 后直接连接，如果仅仅是搜索模式下的，仅仅添加到外设列表数组就可以了。**

```objc
- (void)centralManager:(CBCentralManager *)central didDiscoverPeripheral:(CBPeripheral *)peripheral advertisementData:(NSDictionary<NSString *, id> *)advertisementData RSSI:(NSNumber *)RSSI
{
    // 自动重连发现设备直接连接
    if ([self.connectedMethod isEqualToString:@"1"]) {
        NSString * name = [[NSUserDefaults standardUserDefaults] objectForKey:@"PeripheralName"];
        if ([peripheral.name isEqualToString:name]) {
            
            NSLog(@"自动重连搜索到了 %@ ,正在重连。。。",name);
            self.peripheral = peripheral;
            [central connectPeripheral:peripheral options:nil];
        }
        
    }
    
    // 不是自动重连，则将满足条件的 peripheral 加入到外设列表数组中
    if ([peripheral.name hasPrefix:@"SMART_"]) {
        NSLog(@"DiscoverPeripheral--%@",peripheral.name);
        if (![self.bleArray containsObject:peripheral] ) {
            [self.bleArray addObject:peripheral];
            [[NSNotificationCenter defaultCenter] postNotificationName:@"BleArrayChanged" object:nil];
        }
    }
}
```

##### 步骤 6

**在 `- (void)centralManager:(CBCentralManager *)central didConnectPeripheral:(CBPeripheral *)peripheral` 连接成功代理方法中分别执行对应操作，如果是自动重连，不需要执行相应的界面跳转操作，如果是用户点击 Peripheral 列表连接成功，则需要进行相应界面跳转操作。**

```objc
- (void)centralManager:(CBCentralManager *)central didConnectPeripheral:(CBPeripheral *)peripheral
{
    NSLog(@"--连接成功--%@",peripheral.name);
    
    // 保存外设名字
    [[NSUserDefaults standardUserDefaults] setObject:peripheral.name forKey:@"PeripheralName"];
    
    // 这是重连成功，不进行页面跳转
    if ([self.connectedMethod isEqualToString:@"1"]) {
    
        // 更新外设状态信息
 
    } else if ([self.connectedMethod isEqualToString:@"2"]) {
    
       // 用户点击点击Peripheral 列表连接成功
       
       // 执行相应界面跳转操作
    }
    
    self.peripheral.delegate = self;
    [self.peripheral discoverServices:nil];
      
}

```

##### 步骤 7

**在 `- (void)centralManager:(CBCentralManager *)central didDisconnectPeripheral:(CBPeripheral *)peripheral error:(NSError *)error` 断开连接代理方法中执行对应操作，如果是用户断开连接，则不需要自动重连，如果是其他原因断开连接的，则需要断开连接。**

```objc
// 外设断开连接
- (void)centralManager:(CBCentralManager *)central didDisconnectPeripheral:(CBPeripheral *)peripheral error:(NSError *)error
{
    NSLog(@"--断开连接 -- %@",peripheral.name);
    
    // 更新 外设状态信息
     
    // 用户主动断开连接
    if ([self.disConnectedState isEqualToString:@"3"]) {
        // do nothing
        return;
    }
    
    [self connectToPeripheralOnOneMinutes];
}

```

##### 步骤 8

**在用户主动断开连接的方法中，保存断开方式标志，并断开连接。**

```objc
//断开连接
- (void)StopConnectedButtonDidClicked:(id) sender
{
     // 手动断开方式
    [AppDelegate mainDelegate].disConnectedState = @"3";
    
    [[AppDelegate mainDelegate].centralManager cancelPeripheralConnection:[AppDelegate mainDelegate].peripheral];
    
}
```

*经过以上这些步骤，一个自动重连机制就理清了。*

### 【2】连接后需要配对的情况

**思路**：当你与 Peripheral 配对后，只要你的 Peripheral 在手机的连接范围内，系统会去自动连接你的手环，因此，你需要在连接成功后，保存 Peripheral 的UUIDString。如果想要在 APP 启动后能自动连接之前配对的 Peripheral，你需要执行以下操作。

代码如下：

```objc
- (void)centralManagerDidUpdateState:(CBCentralManager *)central
{
    switch (central.state) {
        case CBCentralManagerStateUnknown:
            NSLog(@">>> CBCentralManagerStateUnknown");
            break;
        case CBCentralManagerStateResetting:
            NSLog(@">>> CBCentralManagerStateResetting");
            break;
        case CBCentralManagerStateUnsupported:
            NSLog(@">>> CBCentralManagerStateUnsupported");
            break;
        case CBCentralManagerStateUnauthorized:
            NSLog(@">>> CBCentralManagerStateUnauthorized");
            break;
        case CBCentralManagerStatePoweredOff:
            NSLog(@">>> CBCentralManagerStatePoweredOff");
            [[NSNotificationCenter defaultCenter] postNotificationName:DisconnectPeripheral object:nil];
            break;
        case CBCentralManagerStatePoweredOn:
        {
           NSLog(@">>> CBCentralManagerStatePoweredOn");
            
            // 1、获取之前配对设备的 UUID String
            NSString * uuidStr = [[NSUserDefaults standardUserDefaults] objectForKey:BleBindUUID];
            NSLog(@" bind uuidStr == %@",uuidStr);
            
            // 2、如果 UUID String 为 nil，说明之前没有配对过任何外设，则外设进行扫描操作，如果不为 nil 且不等于空，则根据 NSUUID 恢复之前配对的 Peripheral
            if (uuidStr != nil && ![uuidStr isEqualToString:@"nil"] && ![uuidStr isEqualToString:@""]) {
                
                // 根据 UUID String 生成 NSUUID 对象
                NSUUID * uuid = [[NSUUID alloc] initWithUUIDString:uuidStr];
                
                // 根据 NSUUID 恢复曾经配对过的设备
                NSArray * peripherals = [central retrievePeripheralsWithIdentifiers:@[uuid]];

                // 数组不为空，说明找到之前配对过的设备
                if (peripherals.count > 0) {
                
                		// 之前配对的 对象
                    CBPeripheral *peripheral = [peripherals firstObject];
                    
                    self.peripheral = peripheral;//**关键**需要转存外设值，才能发起连接
                    
                    self.peripheral.delegate = self;
                    NSLog(@"self.peripheral == %@",self.peripheral);
                   
                    // 60秒自动重连方法
                    [self connectingPeripheralDuringOneMinute];
                }
            }
            else {
            
            		// 开始搜索外设
                [central scanForPeripheralsWithServices:nil options:nil];
                  
                // 4s 后停止扫描
                dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(4 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                    
                   // 停止搜索外设
                });
            }
           
        }
            break;
        default:
            break;
    } 
}
```

*注意：当手环与外设配对连接后，通过 `cancelPeripheralConnection` 方法是有时是无法断开与外设的连接。你需要到系统设置里去忽略这个外设，方可消除配对状态。*

### 结束语

*欢迎在本文下面留言一起交流心得...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*