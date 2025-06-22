# 利用Flutter做成应用3：图片


### 获取网络图片

需要预先将app需要展示的图片存储到网络空间。本应用将图片存储到[cloudinary](https://cloudinary.com/)。  


在pubspec.yaml的dependencies添加[http](https://pub.flutter-io.cn/packages/http)插件。  
```yaml
dependencies:
  http: ^0.13.5
```

注意：  
flutter默认debug版可以进行网络连接，对于release版，需要在android/app/src/main/AndroidManifest.xml中添加网络配置。
```xml
<uses-permission android:name="android.permission.INTERNET" />
```


### 展示图片

通过插件[extended_image](https://pub.flutter-io.cn/packages/extended_image)可以方便的展示网络上的图片。

```yaml
dependencies:
  extended_image: ^6.3.1
```

代码如下所示，显示url指向的的图片，图片显示前显示图片的加载进度。
```dart
  ExtendedImage.network(
      url,
      handleLoadingProgress: true,
      clearMemoryCacheIfFailed: true,
      clearMemoryCacheWhenDispose: true,
      cache: true,
      loadStateChanged: (ExtendedImageState state) {
        if (state.extendedImageLoadState == LoadState.loading) {
          final loadingProgress = state.loadingProgress;
          int? nTotal = loadingProgress?.expectedTotalBytes;
          double? progress;
          if (nTotal != null) {
            progress = loadingProgress!.cumulativeBytesLoaded / nTotal;
          }

          return Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              crossAxisAlignment: CrossAxisAlignment.center,
              children: <Widget>[
                SizedBox(
                  width: 150.0,
                  child: LinearProgressIndicator(
                    value: progress,
                  ),
                ),
                const SizedBox(
                  height: 10.0,
                ),
                Text('${((progress ?? 0.0) * 100).toInt()}%'),
              ],
            ),
          );
        }
        return null;
      },
    );
```

可以对图片有更多的操作，如放大，缩小等。
```dart
  ExtendedImage.network(
      url,
      fit: BoxFit.contain,
      mode: ExtendedImageMode.gesture,
      extendedImageGestureKey: gestureKey,
      initGestureConfigHandler: (ExtendedImageState state) {
        return GestureConfig(
          minScale: 0.9,
          animationMinScale: 0.7,
          maxScale: 4.0,
          animationMaxScale: 4.5,
          speed: 1.0,
          inertialSpeed: 100.0,
          initialScale: 1.0,
          inPageView: false,
          initialAlignment: InitialAlignment.center,
          reverseMousePointerScrollDirection: true,
          gestureDetailsIsChanged: (GestureDetails? details) {
            //print(details?.totalScale);
          },
        );
      },
    ),
```



### 分享图片

通过插件[share_plus](https://pub.flutter-io.cn/packages/share_plus)可将图片分享到社交软件。

```yaml
dependencies:
  share_plus: ^4.5.3
```

代码如下，获取网络上的图片，并借助插件[image](https://pub.flutter-io.cn/packages/image)给图片添加水印后再分享。
```dart
import 'package:image/image.dart' as ui_image;
import 'package:extended_image/extended_image.dart';
import 'package:image_picker/image_picker.dart';
import 'package:share_plus/share_plus.dart';
import 'package:path_provider/path_provider.dart';

Future<void> shareNetworkImage(String url, {bool useCache = true}) async {
  final Uint8List? data = await getNetworkImageData(url, useCache: useCache);
  if (data == null) {
    return;
  }

  ui_image.Image? image = ui_image.decodeImage(data);
  if (image == null) {
    return;
  }

  ui_image.drawString(
      image, ui_image.arial_24, 20, 20, 'avgle.top');

  final Directory temp = await getTemporaryDirectory();
  String name = 'share.jpg';
  final File imageFile = File('${temp.path}/$name');

  imageFile.writeAsBytesSync(ui_image.encodeJpg(image));

  await Share.shareXFiles([XFile(imageFile.path)]);
}
```


### 保存图片

1) 使用插件[image_gallery_saver](https://pub.flutter-io.cn/packages/image_gallery_saver)时，会有如下错误，该插件使用的kotlin-gradle-plugin版本较低，插件很久没更新了，暂不能使用。

```log
* What went wrong:
The Android Gradle plugin supports only Kotlin Gradle plugin version 1.5.20 and higher.
The following dependencies do not satisfy the required version:
project ':image_gallery_saver' -> org.jetbrains.kotlin:kotlin-gradle-plugin:1.3.72
```

2) 插件[gallery_saver](https://pub.flutter-io.cn/packages/gallery_saver)也可以将图片保存到相册。
```yaml
dependencies:
  gallery_saver: ^2.3.2
```
该插件需要将最小的sdk版本设为21以上。
```xml
minSdkVersion 21
```

并且可以直接将网络上的图片保存到相册，网络地址必须以'http/https'开头。
```dart
await GallerySaver.saveImage(url)
```

3) 也可通过插件[photo_manager](https://pub.flutter-io.cn/packages/photo_manager)进行操作，此处不表。

---

> Author:   
> URL: https://yfeier.github.io/tech/build-flutter-app3/  

