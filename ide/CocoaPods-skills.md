### 常用 CocoaPods 命令

| Shortcut Keys | description |
| :--------- | :---------- |
| pod trunk me |  查看自己的 CocoaPods 信息  |
| pod lib lint BaymaxKit.podspec | 检查 lib lint |
| pod trunk push BaymaxKit.podspec --allow-warnings| 发布 podspec 到 CocoaPods|
| pod repo update | 更新本地  CocoaPods 的 spec 资源配置信息仓库|
| gem list --local \| grep cocoapods  | 查看安装的CocoaPods相关东西 |
| sudo gem uninstall cocoapods-core  | 卸载CocoaPods相关东西 |
| sudo gem install -n /usr/local/bin cocoapods  | 安装CocoaPods |
| pod install --verbose --no-repo-update | 安装库和依赖 |
| pod init  | 生成默认的 Podfile配置文件 |

     