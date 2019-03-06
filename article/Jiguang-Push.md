---
title: 关于集成极光推送
date: 2016-11-11 17:40:09
categories: blog
tags: [Objective-C,极光推送]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/11/11/Jiguang-Push](http://muhlenxi.com/2016/11/11/Jiguang-Push)*

#### 前言：

> 通过极光推送服务，主动及时地向您的用户发起交互，向其发送聊天消息、日程提醒、活动预告、进度提示、动态更新等。

学习贵在记录和总结收获！点击阅读全文了解更多！　　

<!-- more --> 

#### 基础篇（集成步骤）

* 1、生成CSR文件

 > 钥匙串访问 -> 证书助理 -> 从证书颁发机构请求证书...
	
* 2、在 [苹果开发者网站](https://developer.apple.com/) 登录后，创建 App ID

 > Account -> Certificates,Identifiers&Profiles -> App IDs -> +

* 3、生成远程推送的测试证书

 *测试证书：*	 
> Development -> Apple Push Notification service SSL (Sandbox)
 
* 4、生成远程推送的生产证书
 
 *生产证书：*
> Production -> Apple Push Notification service SSL (Sandbox & Production)

 **创建好后分别下载到电脑上，然后双击运行。**

* 5、添加真机 Device （添加过的可以忽略）

* 6、创建配置文件 Provisioning Profiles 

 **创建好后下载到电脑上，然后双击运行。**

* 7、将推送证书导出并生成 .p12 文件

 > Apple Development IOS Push Services: 你项目的Bundle identifier 就是在测试环境下的推送证书。
 
 > Apple Push Services: 你项目的Bundle identifier 就是生产环境下的证书.

* 8、在[极光开发者中心](https://www.jiguang.cn/)创建项目并上传刚生成的两个p12文件.

* 9、下载极光 SDK 并配置项目。

  具体配置看[iOS SDK 集成指南](http://docs.jiguang.cn/jpush/client/iOS/ios_guide_new/)

#### 提高篇

*感谢您的阅读，一起学习，一起成长，加油！*