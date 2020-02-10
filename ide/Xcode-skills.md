## Xcode skills

- [Apple Xcode 官方帮助文档](https://help.apple.com/xcode/mac/current/#/devc8c2a6be1)

> 本文记录的是 Xcode 软件的使用技巧。

### Note

-  To open the Archives organizer, choose Window > Organizer, then click Archives.

## Xcode 快捷键 

| Shortcut Keys | description |
| :--------- | :---------- |
| Command + F | 当前文件中查找 |
| Command + B | 编译 |
| Command + R | 运行 |
| Command + . | 停止运行 |
| Command + K | 清理控制台内容 |
| Command + Return | 关闭 Xib 相关联的文件 |
| Command + [ | 左移选中的代码块 |
| Command + ] | 右移选中的代码块 |
| Command + / | 注释或取消注释 代码块 |
| Command + 0 | 显示/隐藏左边导航栏 |
| Command + 1 - 8 | 选择左边导航栏中的选项 |
| Command + ⬅️ | 光标移到本行代码开头 |
| Command + ➡️ | 光标移到本行代码结尾 |
| Command + L | 快速跳到某一行 |
|||
| Control + A | 光标移到本行 行首 |
| Control + E | 光标移到本行 行尾 |
| Control + N | 光标跳到 下一行 |
| Control + P | 光标跳到 上一行 |
|||
| Command + Option + F | 当前文件中查找和替换 |
| Command + Option + 0 | 显示/隐藏右边的导航栏 |
| Command + Option + 1 - 9 | 选择右边导航栏中的选项 |
| Command + Option + / | 给函数添加注释 |
| Command + Option + Return | 打开 Xib 相关联的 ViewController |
| Command + Option + [ | 上移代码块 |
| Command + Option + ] | 下移代码块 | 
|||
| Command + Shift + F | 项目中查找 |
| Command + Shift + Option + F | 项目中查找和替换 |
| Command + Shift + K | 清理缓存 |
| Command + Shift + Y | 显示或隐藏控制台 |
| Command + Shift + O | 根据文件名快速切换文件 |
|||
| Command + Control + space | 呼出 emoji 表情输入 |


## 配置篇

##### Provisioning Profiles 文件目录

```
/Users/muhlenxi/Library/MobileDevice/Provisioning Profiles
```

##### mobileprovision 文件转 plist

```
security cms -D -i 60f769b5-b4ae-4a9a-95dc-b5987e7f067c.mobileprovision > 60f7.plist
```

##### 查找项目中的中文字符串

*如图所示：点击放大镜并切换到 `Find > Regular Expression` 模式下，输入 `@"[^"]*[\u4E00-\u9FA5]+[^"\n]*?"`回车即可！Swift 则要去掉@符号。* 

##### 导出 本地化语言配置 xiff 文件


1、选中项目后，然后点击 `Editor`,选择 红色方框 中的选项，然后执行步骤2。

2、填入文件名和路径，勾选要导出的语言，保存就可以了。


##### pch文件路径

$(SRCROOT)/项目名称/pch文件名


##### Xcode8控制台输出信息过多解决方法

Product -> Scheme -> Edit Scheme... -> Run -> Arguments, 在 Environment Variables 里边添加 OS_ACTIVITY_MODE ＝ disable
