# 利用Flutter做成应用1：环境


## 在Windows上搭建Flutter开发环境

如果访问Flutter受到限制，在环境变量中加入下面的变量
```shell
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

安装Git命令行工具
[Git for Windows ](https://git-scm.com/download/win)


## 获取Flutter SDK

下载  
https://docs.flutter.dev/development/tools/sdk/releases
https://flutter.cn/docs/development/tools/sdk/releases

将安装包zip解压到你想安装Flutter SDK的路径，在Flutter安装目录的flutter文件下找到flutter_console.bat，双击运行并启动flutter控制台。
如果用cmd启动控制台，需要将flutter\bin对应路径添加到环境变量Path中。


Flutter有三个发布渠道，分别为stable、beta 和 master。
如果已经装好Flutter需要升级的话，使用如下命令。
```shell
flutter channel stable
flutter upgrade
```

如果需要更新pubspec.yaml文件里列出的所有依赖的packages，使用如下命令。
```shell
flutter pub upgrade
```

如果需要自动判断那些过时了的 package 依赖以及获取更新建议，现在你可以使用 outdated 命令。
```shell
flutter pub outdated
```

打开一个新的命令提示符或PowerShell窗口并运行以下命令以查看是否需要安装任何依赖项来完成安装：
```shell
flutter doctor
```


## Flutter插件
https://pub.dev/  
https://pub.flutter-io.cn/  


## 开发工具
[Android Studio](https://developer.android.google.cn/studio)

启动Android Studio，安装两个插件（File->Setting->Plugins)
* Flutter插件： 支持Flutter开发工作流 (运行、调试、热重载等).
* Dart插件： 提供代码分析 (输入代码时进行验证、代码补全等).

安装Flutter插件时会同时要求安装Dart插件。


### Tips


```
 flutter doctor --android-licenses
```
 Android sdkmanager not found. Update to the latest Android SDK and ensure that the cmdline-tools are installed to  resolve this.

```
flutter config --android-sdk C:\App\Android\android-sdk
```
Setting "android-sdk" value to "C:\App\Android\android-sdk".
You may need to restart any open editors for them to read new settings.



Error: The proxy server URL extracted from HTTP_PROXY or HTTPS_PROXY environment variable could not be parsed. Either specify the correct URL or unset the environment variable.

```
set HTTP_PROXY=http://ip:port
set HTTPS_PROXY=http://ip:port
set NO_PROXY=localhost,127.0.0.1,::1
```

---

> Author:   
> URL: https://yfeier.github.io/tech/build-flutter-app1/  

