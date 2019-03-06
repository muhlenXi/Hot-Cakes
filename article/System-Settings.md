---
title: iOS 跳转系统设置和打开其他 APP
date: 2017-03-07 17:04:01
tags: [system setting]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/03/07/System-Settings](http://muhlenxi.com/2017/03/07/System-Settings)*

### 导语：

> 在项目中，有时需要跳转到系统设置的某一个界面让用户去设置相关属性。比如一个蓝牙的APP需要检测用户是否打开了蓝牙，否则需要提醒用户并跳转到蓝牙设置界面让用户去打开蓝牙。有时需要打开自带或第三方APP去操作。比如，打开微信、支付宝等。
> 
> 本文 已更新到 Xcode8.0 Swift3.0

　　
<!-- more -->
　　
### 关于适配

通过一张图，我们可以查看到 2017-2-20 号用户的系统版本的分布比例，所以我们只要是配到 9.0 和 9.0 以上就可以满足大部分用户了。[最新系统版本分布图传送门](https://developer.apple.com/support/app-store/)

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/version.png" width="287" height="309" alt="选项列表图"/>
</div>

### 跳转到系统设置

*由于在`iOS 10 以上` 和 `iOS 10 以下`分别是通过不同的方法打开URL的，所以我们要对系统版本做判断操作！*

举例1：跳转到 `系统蓝牙设置` 界面

```objc
let urlStr = "App-Prefs:root=Bluetooth"
if let url = URL(string:urlStr) {
    if #available(iOS 10.0, *) {
        UIApplication.shared.open(url, options: Dictionary(), completionHandler: nil)
    } else {
        // Fallback on earlier versions
        UIApplication.shared.openURL(url)
    }
}

```

### 打开其他APP

举例2：打开 `App Store` 应用

```objc
let urlStr = "itms-apps://"
if let url = URL(string:urlStr) {
    if #available(iOS 10.0, *) {
        UIApplication.shared.open(url, options: Dictionary(), completionHandler: nil)
    } else {
        // Fallback on earlier versions
        UIApplication.shared.openURL(url)
    }
}
```


*常用的第三方应用都定义了不同的 URL Scheme，我们通过 `UIApplication.shared.open()` 方法打开相应的URL，即可跳转到对应的 App 中。iOS10 以下的系统则是使用 `UIApplication.shared.openURL() 方法`）*


举例3：打开 `微信` 应用

```objc
let urlStr = "weixin://"
if let url = URL(string:urlStr) {
    if #available(iOS 10.0, *) {
        UIApplication.shared.open(url, options: Dictionary(), completionHandler: nil)
    } else {
        // Fallback on earlier versions
        UIApplication.shared.openURL(url)
    }
}
```

**了解更多详情，请下载本文 demo 研究！本文的 Demo 在 `10.2.1` 和 `9.3.2` 的真机上都测试过，相关操作均能正常执行！**

### 本文demo

[GitHub传送](https://github.com/muhlenXi/Jump)

### 亲测可以正常使用的 URL Scheme

#### 系统设置

| 要跳转的设置界面  | URL String    | 备注  |
| ------------- |:-------------:| -----:|
| WIFI | App-Prefs:root=WIFI |   |
| Bluetooth | App-Prefs:root=Bluetooth |   |
| 蜂窝移动网络 | App-Prefs:root=MOBILE_DATA_SETTINGS_ID |   |
| 个人热点 | App-Prefs:root=INTERNET_TETHERING |   |
| VPN | App-Prefs:root=VPN |   |
| 运营商| App-Prefs:root=Carrier |   |
| 通知 | App-Prefs:root=NOTIFICATIONS_ID |   |
| 定位服务 | App-Prefs:root=Privacy&path=LOCATION |   |
| 通用 | App-Prefs:root=General |   |
| 关于本机 | App-Prefs:root=General&path=About |   |
| 键盘 | App-Prefs:root=General&path=Keyboard |   |
| 辅助功能 | App-Prefs:root=General&path=ACCESSIBILITY |   |
| 语言与地区 | App-Prefs:root=General&path=INTERNATIONAL |   |
| 还原 | App-Prefs:root=General&path=Reset |   |
| 墙纸 | App-Prefs:root=Wallpaper |   |
| Siri | App-Prefs:root=SIRI |   |
| 隐私 | App-Prefs:root=Privacy |   |
| Safari | App-Prefs:root=SAFARI |   |
| 音乐 | App-Prefs:root=MUSIC |   |
| 照相与照相机 | App-Prefs:root=Photos |   |
| FaceTime | App-Prefs:root=FACETIME |   |
| 电池电量 | App-Prefs:root=BATTERY_USAGE |   |
| 存储空间 | App-Prefs:root=General&path=STORAGE_ICLOUD_USAGE/DEVICE_STORAGE |   |
| 显示与亮度 |App-Prefs:root=DISPLAY |   |
| 声音设置 | App-Prefs:root=Sounds |   |
| App Store | App-Prefs:root=STORE |   |
| iCloud | App-Prefs:root=CASTLE |   |
| 语言设置 | App-Prefs:root=General&path=INTERNATIONAL |   |

#### 自带 App 和第三方 App

| 要打开的APP     | URL Scheme    | Bundle Identifier  |
| ------------- |:-------------:| -----:|
|  |  |  |
| 打10086 | tel://10086 |  |
| App Store | itms-apps:// |  |
| Safari | http://muhlenxi.com/ |  |
| Maps | maps:// |  |
| 备忘录 | mobilenotes:// |  |
| SMS | sms:// |  |
| Mail | mailto:// |  |
| iBooks | ibooks:// |  |
| Music |  music:// |  |
| Videos | videos:// |  |
|  |  |
| QQ | mqq:// |  |
|微信 | weixin:// ||
| 淘宝 | taobao://||
| 支付宝 | alipay:// | |
| 新浪微博 | sinaweibo:// | |
|知乎| zhihu://| |

### 后记

*感谢阅读，有什么问题可以给我留言。如果觉得不错的话，可以点个 Star 支持一下作者！本文不阶段更新中...*
