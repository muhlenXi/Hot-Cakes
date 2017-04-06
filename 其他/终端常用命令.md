### RubyGems

*添加源*

    gem sources --add https://gems.ruby-china.org/ 

*移除源*

    gem sources --remove https://rubygems.org/  

*更新gem*

    sudo gem update -n /usr/local/bin --system 

*查看gem版本*

    gem -v 

### CocoaPods


*查看安装的CocoaPods相关东西*

    gem list --local | grep cocoapods 

*卸载CocoaPods相关东西*

    sudo gem uninstall cocoapods-core 

*安装CocoaPods*

    sudo gem install -n /usr/local/bin cocoapods 

*安装库和依赖*

    pod install --verbose --no-repo-update 

*生成默认的cocoapods配置文件*

    pod init 

*使用Xcode打开文件*

    open -a Xcode AlamoFireDemo.xcworkspace 

### 常用快捷键

*不同程序之间切换*

    Command + Tab  

*呼出大部分应用的查询功能*

    Command + F 

*复制*

    Command + C 

*粘贴*

    Command + V 

*剪切*
    Command + X 

*后退*
    
    Command + Z 

*前进*

    Command + shift + Z 

*新建程序应用窗口*

    Command + N 

*退出当前应用程序*

    Command + Q 

*切换输入法*
   
    Command + space 

### 终端常用命令

 *显示mac中隐藏文件(需重启Finder)*
   
    defaults write com.apple.finder AppleShowAllFiles -bool true 
         
*隐藏mac中隐藏文件(需重启Finder)*
 
    defaults write com.apple.finder AppleShowAllFiles -bool false
   
*关闭Finder*  &  *打开Finder* 

    killall Finder 
    open . 
    
*删除文件夹及其内容* 

    rm -rf 文件夹名  
    
*获取电脑IP地址*

    ifconfig en0 

*查看当前路径*

    pwd 

*查看当前路径下的目录和文件*

    ls 

*查看文件和目录下的详细信息*

    ls -l 

*查看所有文件，包括隐藏文件*

    ls -a 

*进入目录*

    cd 

*返回上级目录*

    cd .. 

*返回根目录*

   cd ~ 


   
### 安装wget工具

终端执行步骤：

Homebrew是一款非常强大的可以应用在MAC中的Linux管理包。

*A - 执行安装brew*

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
*B - 安装 wget*

    brew install wget
    
    
### 生成 UUID

    uuidgen