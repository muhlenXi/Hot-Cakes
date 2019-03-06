---
title: iOS Add PCH File
date: 2017-03-09 16:23:38
tags: [PCH File]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2017/03/09/iOS-Add-PCH-File](http://muhlenxi.com/2017/03/09/iOS-Add-PCH-File)*

### 导语：

> pch 文件是APP项目中的一个头文件，该文件中的内容能被项目中的其他源文件共享和访问，是一个预编译文件。
> 
> 软件版本：Xcode 8.2.1

　
<!-- more -->
　　
### pch文件的作用

* 存放一些全局的宏定义。
* import 整个项目常用的头文件。

### pch文件的添加步骤

*按顺序执行下面的4个小步骤就可以了。*

#### 创建pch头文件

*步骤一：在项目中 选中 `Supporting Files` 分类然后 `Command+N` ，在打开的窗口中往下滚动，选择一个 `PCH File` 的文件，点 Next ，输入 `文件名（建议用项目名）`并确定。*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/pch1.png" width="730" height="518" alt="选项列表图"/>
</div>

#### 配置pch头文件的路径

*步骤二：选择TARGETS -> Build Settings ，在 查找框中输入 `Prefix` 回车进行查找。如图：*

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/pch3.png" width="730" height="170" alt="选项列表图"/>
</div>

*步骤三：查找结果如下图，将 `Precompile Prefix Header` 的值改为 `Yes` ，用以缓存编译后的pch文件，提高编译效率。然后在`Prefix Header` 中输入 `$(SRCROOT)/项目名/项目名.pch`,然后回车即可*。

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/pch4.png" width="724" height="220" alt="选项列表图"/>
</div>

#### 重新编译项目

*步骤四：在项目中，使用 `Command+B` 即可。*

### 后记

*感谢阅读，有什么问题可以给我留言。*
