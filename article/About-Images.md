---
title: iOS开发图片那些事儿
date: 2016-09-30 14:17:09
categories: 开发备忘
tags: [iOS开发图片]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/09/30/About-Images](http://muhlenxi.com/2016/09/30/About-Images)*

### 前言：

> APP 开发中，一些 UI 界面的设计常常涉及到 UIImageView 的使用，这和图片是分不开的，随着开发的渐渐深入，要记住的东西越来越多，但我是一个懒人，暂且就把关于图片的知识就放这儿吧。。

  点击阅读全文来了解一下详情吧。

<!-- more -->

### iPhone 尺寸规格

| 设备(iPhone) | 对角线(Diagonal) | 逻辑分辨率(point)| 设备分辨率(pixel) | Scale Factor|
| -----  |:--------:| -------:|:--------:| ---:|
| 3GS    | 3.5-inch | 320x480 |320x480   | @1x |
| 4/4S   | 3.5-inch | 320x480 |640x960   | @2x |
| 5/5S/5C| 4-inch   | 320x568 |640x1136  | @2x |
| 6/6S   | 4.7-inch | 375x667 |750x1334  | @2x |
| 6plus  | 5.5-inch | 414x736 |1242x2208 1080x1920 | @3x |


### App Logo 尺寸规格

**logo：适配 iOS7.0 and Later**

|   logo命名 | 图片规格  | 分辨率    | 
| -----     |:--------:| -------: |
| logo20@2x.png | @2x      | 40x40    |
| logo20@3x.png | @3x      | 60x60    |
| logo29@2x.png | @2x      | 58x58    |
| logo29@3x.png | @3x      | 87x87    |
| logo40@2x.png | @2x      | 80x80    |
| logo40@3x.png | @3x      | 120x120  |
| logo60@2x.png | @2x      | 120x120  |
| logo60@3x.png | @3x      | 180x180  |
| logoAppstore@2x.png | @2x      | 1024x1024  |

### App LaunchImage 尺寸规格

**launchImage：适配 iOS7.0 and Later**

**launchImage：适配 iOS8.0 and Later**


|   launchImage命名   | 图片规格    | 分辨率      | 
| --------------     |:--------:| ----------: |
| launchImage960@2x.png  | @2x      | 640x960     |
| launchImage1136@2x.png | @2x      | 640x1136    |
| launchImage1334@2x.png | @2x      | 750x1334    |
| launchImage2208@3x.png | @3x      | 1242x2208   |

### App 初次安装引导页 尺寸规格

**launchImage：适配 iOS7.0 and Later**

|   launchImage命名   | 图片规格    | 分辨率      | 
| --------------     |:--------:| ----------: |
| guide4@2x.png  | @2x      | 640x960     |
| guide5@2x.png  | @2x      | 640x1136    |
| guide6@2x.png  | @2x      | 750x1334    |
| guide6P@3x.png | @3x      | 1242x2208   |


### App 启动广告页 尺寸规格

**launchImage：适配 iOS7.0 and Later**

|   launchImage命名   | 图片规格    | 分辨率      | 
| --------------     |:--------:| ----------: |
| ad4@2x.png  | @2x      | 640x960     |
| ad5@2x.png  | @2x      | 640x1136    |
| ad6@2x.png  | @2x      | 750x1334    |
| ad6P@3x.png | @3x      | 1242x2208   |


*本文会不间断更新，如有问题，可以留言给我！最后谢谢阅读！*

