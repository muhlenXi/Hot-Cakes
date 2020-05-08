
[fastlane 简介](https://fastlane.tools/)

安装 fastlane

```shell
sudo gem install -n /usr/local/bin fastlane --verbose
```

查看 fastlane 版本号

```shell
fastlane -v
```

安装蒲公英的 Fastlane 插件

```shell
fastlane add_plugin pgyer
```

match 证书描述文件配置

```shell
fastlane match development --readonly true

fastlane match adhoc --readonly true

fastlane match appstore --readonly true
```
