---
title: 在 Mac 上如何搭建 Hexo 个人博客
date: 2015-08-01 11:11:17
categories: blog
tags: [Hexo,Next]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2015/08/01/How-to-make-a-blog-by-Hexo/](http://muhlenxi.com/2015/08/01/How-to-make-a-blog-by-Hexo/)*

### 导语：

> 在网上经常浏览技术牛人的博客来学习技术能力，突然有个念头闪现在脑海,为啥我不自己也弄个博客记录自己的学习收获和分享自己的掉坑经验，避免后来的人不像我一样掉坑，岂不更好！对于一个爱折腾的我，立马就开始各种搜索资料，各种踩坑。
    
　　更多详情请看[我为什么开始写博客]()

<!-- more -->

　　对于不了解Hexo是什么的？可以去Hexo官网了解一下。[Hexo简介](https://hexo.io/zh-cn/) ,废话不扯了，开始正题!

### 安装Node.js（必须）

　　选择 Node.js 的安装的程序，进入到下载页面，选择 `Download for OSX(x64)`,左边的版本是建议大多数用户使用的版本，右边的则是最新版本，我选择了左边的那个版本，然后，下载，安装即可。[传送门](https://nodejs.org/en/)
  
### 安装Git(必须)

　　由于我经常使用 `Xcode` 软件，所以这里不用安装 Git,Xcode 软件自带 Git,也许有些人不知道 `Git` 是什么以及怎么使用，可以看[Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)了解一下，这是我目前见到讲解的深入浅出的文章。

　　PS：后面需要配置站点和主题文件，需要打开其他格式的文本，个人建议用Xcode会好些。

　　[Xcode for Mac 安装](https://itunes.apple.com/cn/app/xcode/id497799835?mt=12&ign-mpt=uo%3D2)

　　[Git 安装](http://mac.softpedia.com/get/Developer-Tools/Git.shtml#download)
  
### 注册Github账号（必须）
　　注册`Github账号`可以看[Github的注册与使用（详细图解）](http://m.blog.csdn.net/article/details?id=51336332)

　　进一步想深入学习Github的可以看一下这个博客 [从0开始学习Github系列汇总](http://stormzhang.com/github/2016/06/19/learn-github-from-zero-summary/)
### Hexo 的安装与配置
#### 安装Hexo
　　以上三步，根据自己实际情况安装。安装完成后，可进行接下来这步。打开`终端`，cd到你想要存放Hexo配置的路径下进行如下操作：

    mkdir "MyBlog"     //创建MyBlog文件夹
    ls                 //查看当前目录下是否有MyBlog文件夹
    cd cd MyBlog/      //进入到MyBlog目录下
    
    npm install hexo-cli -g  //安装Hexo
    hexo init                //初始化Hexo
    
　　到这里为止，Hexo博客安装工作基本完成，`MyBlog`就是你博客的根目录，关于博客的所有操作均在MyBlog里面进行。

#### 配置Github

　　【1】登录你的Github账号，创建一个`Repository（仓库）`，仓库的名字必须是 `你的github的用户名.github.io`  （这是固定写法）

　　*在这里假设你的用户名是`zhangsan`，则你要创建的仓库名就是`zhangsan.github.io`*

　　【2】用 `终端` 或者 `Github Desktop`软件 Clone zhangsan.github.io 仓库到本地你指定的目录下。
    
　　【3】进入到`MyBlog`文件夹，找到 `_config.yml` (站点配置文件)然后打开它，我用`Xcode`打开它，翻到最后面，进行如下操作

　　改成这个样子：

    deploy:
      type:git
      repository:git@github.com:zhangsan/zhangsan.github.io.git
      branch:master
      
　　【4】打开`终端` 输入 

     npm install hexo-deployer-git --save 
     hexo generate 或 hexo g    //生成静态界面
     hexo server                //本地启动
　　【5】打开`浏览器` 输入 `http://localhost:4000/` 则可以看到你 本地生成的静态页面了

　　打开终端 使用快捷键 `control + c` 停止本地启动进程

　　【6】拷贝`MyBlog`目录下的`Public`文件夹里面的所有文件到你Clone到本地的`zhangsan.github.io` 文件夹 

　　【7】使用 `终端` 或者 `Github Desktop`  Commit、Push `zhangsan.github.io` 到远程Github仓库中
 
　　【8】使用 `终端` 进入到 `MyBlog`目录下，执行如下命令

    hexo deploy  //部署博客到Github
    
　　这样，就成功的将你的Hexo博客部署到了Github，你在浏览器中输入http://zhangsan.github.io 就可以了看到你的博客了。

#### 常用的Hexo基本操作

    hexo clean                //清理缓存
    hexo generate             //生成静态界面
    hexo deploy               //部署到Github
    hexo server               //本地启动博客
    
    hexo new “文章名”          //新建一篇文章
    hexo new page "页面名"     //新建一个页面
    hexo help                 //查看帮助
    hexo version              //查看Hexo的版本

### 给你的博客安装并配置 Next主题

　　Next主题的主旨在于简洁优雅并且易于使用，尤其是是`精于心，简于形`的里面深入我心，见到的第一眼，就深深的吸引住了，无法自拔。具体步骤如下：

　　【1】使用`终端` 进入到 `MyBlog`目录下，输入命令
    
    git clone https://github.com/iissnan/hexo-theme-next themes/next

　　这样就把Next主题Clone到你的`MyBlog/themes`路径下了。

　　【2】进入到`MyBlog`文件夹，找到 `_config.yml` (站点配置文件)然后打开它，我用Xcode打开，找到`theme`字段，修改成

    theme: next
    
　　【3】配置Next主题，进入到`MyBlog/themes/next`目录下，打开`_config.yml` (主题配置文件)然后打开它，我一般用Xcode打开，找到`Schemes`字段，next主题有三种形式，我中意`Mist`,修改如下

    # Schemes
    #scheme: Muse
     scheme: Mist
    #scheme: Pisces
    
　　【4】打开`终端`进入到cd到`MyBlog`文件夹,输入命令

    hexo clean
    hexo generate
    hexo server
    
　　这样，打开`浏览器` 输入 http://localhost:4000/ 就可以看到和我一样的主题了。

　　*我的博客* [mulenxi](http://muhlenxi.com) 

　　更多的Next主题配置，可以参考[Next官方文档](http://theme-next.iissnan.com/getting-started.html)。 

### 搭建博客参考 

* 1、购买域名，对域名解析和绑定Github，可以参考[如何搭建一个独立博客](http://cnfeat.com/blog/2014/05/10/how-to-build-a-blog/)
* 2、学习使用`Markdown`,可以参考[认识与入门Markdown](http://sspai.com/25137)
* 3、Hexo搭建过程出现问题，可以参考[Hexo---搭建](http://www.jianshu.com/p/a2023a601ceb)

