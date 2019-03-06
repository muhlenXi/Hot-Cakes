---
title: zsh 的配置和 CocoaPods 的安装与使用
date: 2015-10-05 16:17:11
categories: blog
tags: [zsh,CocoaPods]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2015/10/05/zsh-and-CocoaPods](http://muhlenxi.com/2015/10/05/zsh-and-CocoaPods)*

### 导语：

> 对于一个爱折腾的人是闲不住的，最近经常和`终端(terminal)`打交道，看着这普普通通的界面，实在在是人不可忍了，有一次网上查资料的时候，了解到`zsh`可以配制出高逼格的用户界面，就是配置有些复杂。但是程序猿都是一群聪明的家伙，研究出`oh-my-zsh`这个开源项目，让zsh配置降到了零门槛，而且完全兼容`bash`。

　　
　　废话不多说，开整吧！
　　
　　<!-- more -->
　　
### 通过oh-my-zsh配置zsh

#### 配置步骤

　　打开我们亲爱的`终端`：输入命令：cat /etc/shells

　　可以看到Mac内置了6中shell
　　

![终端1](http://7xvffo.com1.z0.glb.clouddn.com/%E7%BB%88%E7%AB%AF1.png)

　　

**1、安装 oh-my-zsh ，它会自动读取你的环境并帮你设置zsh **

　`oh-my-zsh` 的地址是：`http://github.com/robbyrussell/oh-my-zsh`

　　在终端内输入以下命令clone oh-my-zsh：

    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh  
    
**2、替换zshrc文件：**

    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
    
**3、切换到zsh模式：**

    chsh -s /bin/zsh
    
**4、关闭并重新打开终端后，会发现变成zsh了，接下来是选择一款高逼格的主题！**

　　oh-my-zsh [主题](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)的主题和对应截图在在这里！

　　我选择的是ys主题，在终端中打开oh-my-zsh的配置文件，设置主题为ys：

    vim ~/.zshrc  //打开.zshrc文件
    
　　按`字母i`进入编辑模式，修改`ZSH_THEME="ys"`后，按`esc`退出编辑模式，再按`shift + ：`，输入`wq`,就是保存并退出的意思。

![ys](http://7xvffo.com1.z0.glb.clouddn.com/ys.png)

**5、打开`终端`的`偏好设置`，设置如下，就和我的一样了：**

![ysset](http://7xvffo.com1.z0.glb.clouddn.com/zsh%E8%AE%BE%E7%BD%AE.png)

　　*zsh的配置到这里就结束了*。
    
　　*附上我的zsh最终效果图：*

![效果图](http://7xvffo.com1.z0.glb.clouddn.com/zsh%E6%95%88%E6%9E%9C%E5%9B%BE.png)
    
#### 终端常用的命令：

     1、clear						清除屏幕或窗口内容
     2、ls							显示当前目录的内容
     3、ls -ah					全部显示当前目录的内容（包含隐藏的）
     4、mkdir						创建一个目录    如：mkdir  helloword
     5、rmdir						删除一个目录
     6、mvdir						移动或重命名一个目录
     7、cd							改变当前目录
     8、pwd						显示当前目录的路径名
     9、dircmp					比较两个目录的内容 
     10、killall -KILL Finder	重启Finder
     11、cd ..					返回上一级目录
     12、cd ~						返回主目录
     
     
 * `显示mac中隐藏文件(需重启Finder)`
   
   defaults write com.apple.finder AppleShowAllFiles -bool true 
         
 * `隐藏mac中隐藏文件(需重启Finder)`
 
   defaults write com.apple.finder AppleShowAllFiles -bool false
         
 　　*后续会陆续添加...*


### CocoaPods的安装与使用

#### CocoaPods的安装

　　CocoaPods是一个第三方库的依赖管理工具，可以自动更新第三方库，自动添加系统依赖库，自动设置编译选项，总的来说，就是能自动配置第三个方库的运行环境。他是一个命令行工具！

　　打开`终端`如下操作

**1、移除默认的ruby源**

    gem sources --remove https://rubygems.org/
    
　　移除后会提示：`https://rubygems.org/ removed from sources`

**2、添加 taobao 的源**

    gem sources -a https://ruby.taobao.org/
    
*目前源已经更新为：https://gems.ruby-china.org 2017-03-21 修正*

* 1、*如果曾经添加过淘宝的源，请执行如下操作，确保只有一个源。*
    
`gem sources --add https://gems.ruby-china.org/ --remove http://ruby.taobao.org/`
    
* 2、*如果没有添加过 taobao 的源，可直接 安装 ruby-china 的源* 
    
`gem sources --add https://gems.ruby-china.org/`
    
    
添加后会提示相应的源已经 added to sources。
 
**3、查看当前的源**

    gem sources -l
    
 　　显示结果如图：（该图已经过期，现在应该显示的是 https://gems.ruby-china.org/）
 
 ![yuan](http://7xvffo.com1.z0.glb.clouddn.com/%E6%B7%98%E5%AE%9D%E6%BA%90.png)
 
 **4、安装CocoaPods**
 
     sudo gem install -n /usr/local/bin cocoapods
     
 　　安装完成如下图：
 
 ![finish](http://7xvffo.com1.z0.glb.clouddn.com/CocoaPods%E5%AE%89%E8%A3%85.png)
 
 　　`终端`输入`gem list`查看CocoaPods的版本
 
 ![hehe](http://7xvffo.com1.z0.glb.clouddn.com/gemlist.png)
 
 　　*由图知，目前CocoaPods的版本是`1.0.1`*
 
#### CocoaPods 添加第三方库

**1、查看一个库是不是支持CocoaPods** 以AFNetworking为例

    pod search AFNetworking
    
 　　出现如图信息则表示`支持`
 
 ![](http://7xvffo.com1.z0.glb.clouddn.com/AFNEtworking.png)
 
 **2、在Podfile文件添加相应版本的库**
 
 　　cd到你的Xcode项目的目录下：
 
     vim Podfile
     
 　　在里面添加
 
 ![podfile](http://7xvffo.com1.z0.glb.clouddn.com/podfile.png)
 
 `说明：`
 
 　　Podfile升级之后到1.0.0版本，Pod里的内容必须明确指出所用第三方库的target
 
 　　所以在podfile文件需要明确：
 
      target “YOUR_TARGRT_NAME” do   
      ...
      end
  
  
 **3、安装第三方库**   
 
     pod install --verbose --no-repo-update
     
 　　安装完成显示如图：
 
 ![state](http://7xvffo.com1.z0.glb.clouddn.com/state.png)
 

**4、CocoaPods更新第三方库**

　　当我们拿到别人的项目，或者自己的项目要添加新的第三方库，需要更新第三方库。

    pod update --verbose --no-repo-update
    

　　*感谢阅读，有什么问题可以给我留言。*

