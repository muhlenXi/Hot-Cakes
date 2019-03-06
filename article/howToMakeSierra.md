### 制作 macOS Sierra 系统装机启动盘

> 重要的事情说三遍： 装系统前记得备份所有数据！装系统前记得备份所有数据！！装系统前记得备份所有数据！！！

### 本机环境

* macOS Sierra 版本 10.12.5

### 制作启动盘步骤

##### 1、格式化U盘

1-1、打开 `磁盘工具` 选择要格式化的 U 盘，点击 `抹掉`。

1-2、在弹出的选项中，`名称`为 `Sierra`,`格式` 为 `OS X 扩展（日志式）`，`方案`为`GUID 分区图`。

1-3、点击抹掉，如果失败，则进行重试，直到提示成功为止。

##### 2、下载 mac OS Sierra

2-1、在 `App Store` 中搜索并下载 `mac OS Sierra` app。

##### 3、使用终端制作启动盘

3-1、打开 `终端`,复制并粘贴以下指令，然后回车后输入开机密码后等待命令执行完毕。

要复制的指令：

```objc
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Sierra  --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction
```

程序会陆续会提示以下的信息，如果最终结果如下所示，当你看到`Done`时就表示启动盘制作完成。

```objc
Erasing Disk: 0%... 10%... 20%... 30%...100%...
Copying installer files to disk...
Copy complete.
Making disk bootable...
Copying boot files...
Copy complete.
Done.
```

### 如何用启动盘安装系统

1-1、插上刚制作好的启动盘，重启 mac 然后一直按着 `option 或 alt` 不放，直到出现提示选择启动盘的界面。

1-2、选择右边的选项，也就是 `Install macOS Sierra`。

1-3、然后就是漫长的等待了...


