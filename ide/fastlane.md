
[fastlane 简介](https://fastlane.tools/)

### 安装

```shell
sudo gem install -n /usr/local/bin fastlane --verbose
```


### 证书描述文件配置

```shell
fastlane match development --readonly true

fastlane match adhoc --readonly true

fastlane match appstore --readonly true
```
