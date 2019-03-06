---
title: App Frameworks 之 Foundation 框架
date: 2017-01-20 10:20:19
categories: blog
tags: [Foundation]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: <http://muhlenxi.com/2017/01/20/About-Foundation>*

### 导语：

> 学习软件开发最好的方式就是去努力学好英语，然后去看官方的第一手资料 - 软件开发指南。
> 本文是关于 Foundation 框架的描述。

<!-- more -->

如果你想要学 mac OS / iOS App 开发，你需要参考的第一手资料则是 [Apple Developer Documentation](https://developer.apple.com/documentation),在这里，Apple 公司会不定期更新最新的 Kit(开发包)，包括 API reference（API 接口）、技术文章和示例代码等。


### Apple 开发的基础框架

Apple App 开发涉及到的（Framework）基础框架主要有 6 大部分，分别如下：

* [AppKit](https://developer.apple.com/documentation/appkit)
* [Foundation](https://developer.apple.com/documentation/foundation)
* [Objective-C](https://developer.apple.com/documentation/objectivec)
* [Swift Standard Library](https://developer.apple.com/documentation/swift)
* [UIKit](https://developer.apple.com/documentation/uikit)
* [WatchKit](https://developer.apple.com/documentation/watchkit)

博客稍后会对每个类别做一个简介，不定时更新，如果感兴趣想深入了解，可以点击链接跳转到官网学习。

#### Foundation

Foundation 是一个基础的框架，用于支持你的 App 的基础功能定义，比如基本数据类型、数据集合和操作系统服务的访问。

Foundation 框架为你的 App 和其他框架的基础功能提供支持，包括数据的保存和持久化存储，文本处理、日期和时间的计算、排序和过滤，网络等。mac OS，iOS，watchOS，以及 tvOS 的 SDK（软件开发包中）都有使用到 Foundation 框架中定义的类、协议和数据类型等。

##### Foundation 基础

###### Numbers,Data,和 Basic Value

这部分主要是包含 Cocoa 中使用的基础数据类型。[详情传送门](https://developer.apple.com/documentation/foundation/numbers_data_and_basic_values)

包括：

**struct** : `Int`,`Double`,`Decimal`,`Data`,`URL`,

`URLComponents`,`URLQueryItem`,`UUID`,`CGFloat`,`AffineTransform`,

`NSEdgeInsets`

**class** :  `NumberFormatter`,

**typealias** : `NSPoint`,`NSSize`,`NSRect`,`NSRange`

###### Strings 和 Text

这部分主要包括 Unicode 字符串的创建和处理，使用正则表达式模式查找以及文本的自然语言分析。[详情传送门](https://developer.apple.com/documentation/foundation/strings_and_text)

包括：

**struct** : `String`,`CharacterSet`,`Locale`

**class** : `NSAttributedString`,`NSMutableAttributedString`,`NSLinguisticTagger`,`Scanner`,`NSRegularExpression`,

`NSDataDetector`,`NSTextCheckingResult`,`NSSpellServer`,`NSOrthography`

**typealias** : `UnicodeScalar`

**let** : `NSNotFound`

**protocol** : `NSSpellServerDelegate`

###### Collections

这部分主要包括 array，set，dictionary 和 专用集合来存储和遍历集合中的对象或者值。[详情传送门](https://developer.apple.com/documentation/foundation/collections)

包括：

**struct** : `Array`,`Dictionary`,`Set`,	`IndexPath`,`IndexSet`,

`NSFastEnumerationIterator`,`NSIndexSetIterator`,`NSEnumerationOptions`,`NSSortOptions`

**class** : `NSCountedSet`,`NSOrderedSet`,`NSMutableOrderedSet`,`NSCache`,`NSPurgeableData`,

`NSPointerArray`,`NSMapTable`,`NSHashTable`,`NSEnumerator`,`NSNull`

**protocol** : `NSFastEnumeration`

**let** : `NSNotFound`

###### Dates 和 Times

这部分主要包括日期和时间的比较、日历和时间域的计算。[详情传送门](https://developer.apple.com/documentation/foundation/dates_and_times)

**struct** : `Date`,`DateInterval`,`DateComponents`,`Calendar`,`TimeZone`,`Locale`

**typealias** : `TimeInterval`

**class** : `DateFormatter`,`DateComponentsFormatter`,`DateIntervalFormatter`,`ISO8601DateFormatter`


###### Units 和 Measurement

这部分主要是物理参数的测量和单位转换。[详情传送门](https://developer.apple.com/documentation/foundation/units_and_measurement)

**struct** : `Measurement`

**class** `Unit`,`Dimension`,`UnitConverter`,`UnitConverterLinear`,`UnitArea`,

`UnitLength`,`UnitVolume`,`UnitAngle`,`UnitMass`,`UnitPressure`,

`UnitAcceleration`,`UnitDuration`,`UnitFrequency`,`UnitSpeed`,`UnitEnergy`,

`UnitPower`,`UnitTemperature`,`UnitIlluminance`,`UnitElectricCharge`,`UnitElectricCurrent`,

`UnitElectricPotentialDifference`,`UnitElectricResistance`,`UnitConcentrationMass`,`UnitDispersion`,`UnitFuelEfficiency`
 
###### Data Formatting

主要是 number, date, measurement 以及一些其他值的文本转换。[详情传送门](https://developer.apple.com/documentation/foundation/data_formatting)

**class** : `NumberFormatter`,`PersonNameComponentFormatter`,`DateFormatter`,`DateComponentsFormatter`,`DateIntervalFormatter`,

`ISO8601DateFormatter`,`ByteCountFormatter`,`MeasurementFormatter`,`Formatter`,`LengthFormatter`,

`MassFormatter`,`EnergyFormatter`

**struct** : 	`PersonNameComponents`,`Locale`

###### Filter 和 Sorting

主要包含使用 predicate（谓词），expression（表达式）和 sort descriptors（排序描述）来过滤和排序集合中的元素。[详情传送门](https://developer.apple.com/documentation/foundation/filters_and_sorting)

**class** : `NSPredicate`,`NSExpression`,`NSComparisonPredicate`,`NSCompoundPredicate`,`NSSortDescriptor`

**enum** : `ComparisonResult`



### 结束语

*欢迎在本文下面留言一起交流心得或错误指正...*

*如果本文能给你带来一定的帮助，在自己有能力的情况下，不妨赞助一下，表示对博主辛勤耕作的支持！*
    




