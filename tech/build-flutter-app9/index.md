# 利用Flutter做成应用9：图标与启动画面


### APP启动图标

使用插件[flutter_launcher_icons](https://pub.flutter-io.cn/packages/flutter_launcher_icons)来做成App的启动图标。

可以根据给定的图片自动生成不同分辨率的应用图标。


```yaml
dev_dependencies:
  flutter_launcher_icons: ^0.10.0

flutter_icons:
  android: "ic_launcher"
  ios: true
  remove_alpha_ios: true
  image_path: "assets/imgs/launcher1.png"
  min_sdk_android: 21 # android min sdk min:16, default 21
  windows:
    generate: true
    image_path: "assets/imgs/launcher1.png"
    icon_size: 256 # min:48, max:256, default: 48
```

android:ic_launcher表示生成android平台的应用图标的名称
ios:false表示不生成ios的图标
image_path为源图片的路径


在项目文件夹下面运行命令生成图标。

```cmd
flutter pub get
flutter pub run flutter_launcher_icons:main
```


#### Tips
如何获取icon图片

* 以下网站包含大量icon图片

https://thenounproject.com/

该网站图片可以免费下载，提供png和svg两种格式，免费下载的话图片上会包含多余的信息。

图片可以进行一些简单旋转，颜色调整。


如不想花钱下载，按F12找到图片对应的\<img src=""\>该段代码，将src中的内容复制出来，里面是图片的base64编码。



* 通过下面的网址，将上面复制的base64编码贴入并转换成图片。图片另存下来，是svg格式。

https://tool.jisuapi.com/base642pic.html



* 通过下面的网址，将上面的svg图片转成png格式。

https://svgtopng.com/zh/



* 如果上面的png图片需要调整尺寸，通过下面的网址可以进行调整。

https://www.iloveimg.com/zh-cn/resize-image/resize-png#resize-options,pixels



* 获得png图像后，由于android需要不同尺寸的图标，可以通过下面的网址生成所有尺寸的图标。

https://icon.wuruihong.com/#/android


### 启动画面

使用插件[flutter_native_splash](https://pub.flutter-io.cn/packages/flutter_native_splash)来生成App的启动画面。

```yaml
dependencies:
  flutter_native_splash: ^2.2.10+1

flutter_native_splash:
  color: "#000000"
  image: "assets/imgs/splash.png"
  fullscreen: true
  android_gravity: center
  ios_content_mode: center
```

在项目文件夹下面运行命令生成启动画面。
```cmd
flutter pub run flutter_native_splash:create
```


默认情况下启动画面会在绘制第一个frame时自动消除。也可以手动消除，使App在初始化时显示启动画面，在初始化完成后再手动消除启动画面。

```dart
import 'package:flutter_native_splash/flutter_native_splash.dart';

void main() {
  WidgetsBinding widgetsBinding = WidgetsFlutterBinding.ensureInitialized();
  FlutterNativeSplash.preserve(widgetsBinding: widgetsBinding);
  runApp(const MyApp());
}

// whenever your initialization is completed, remove the splash screen:
    FlutterNativeSplash.remove();
```



---

> Author:   
> URL: http://localhost:1313/tech/build-flutter-app9/  

