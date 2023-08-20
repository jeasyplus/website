# Flutter

## 环境安装

### Flutter

[下载安装](https://flutter.cn/docs/get-started/install)

添加 PATH 环境变量
```shell
export PATH="$PATH:Flutter安装目录/flutter/bin"
```
### Xcode

[下载安装](https://developer.apple.com/xcode/download/)

配置 Xcode 开发者路径
```shell
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
sudo xcodebuild --license
```
安装 CocoaPods
安装前更新镜像源：
```shell
gem update --system

gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
或者
gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
```
```shell
sudo gem install cocoapods --user-install --verbose

pod --version
```

### Android Studio

[下载安装](https://developer.android.google.cn/studio)

## jdk
安装jdk对应版本(如:17)，然后检查
```shell
flutter doctor --android-licenses
```

### Chrome

[下载安装](https://www.google.com/chrome/next-steps.html)

### 环境检查

运行 flutter doctor
```shell
flutter doctor
```

