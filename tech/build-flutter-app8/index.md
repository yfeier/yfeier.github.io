# 利用Flutter做成应用8：Google广告



### Google广告

* 在下面网站上先设置一个新应用。  
https://apps.admob.com

创建应用后可以获取应用ID，并在AndroidManifest.xml中添加以下字段。
```xml
<meta-data
    android:name="com.google.android.gms.ads.APPLICATION_ID"
    android:value="***app id***" />
```

后续步骤：  
1) 创建广告单元并测试 SDK 集成  
2) 完成测试后，请将您的应用发布到受支持的应用商店  
3) 将商店添加到您的 AdMob 应用中。我们会对您的应用进行一些检查，确保它可以展示广告。评估您的应用通常需要几天时间，但在某些情况下可能需要更长时间。

* 创建广告单元
可创建横幅广告，激励广告和原生高级广告等。创建后可以获取广告id，用于app中来展示广告。


### 展示广告

使用插件[google_mobile_ads](https://pub.flutter-io.cn/packages/google_mobile_ads)来在应用中展示广告。

```yaml
dependencies:
  google_mobile_ads: ^2.0.1
```

*Android广告测试ID*
```
开屏广告	ca-app-pub-3940256099942544/3419835294
横幅广告	ca-app-pub-3940256099942544/6300978111
插页式广告	ca-app-pub-3940256099942544/1033173712
插页式视频广告	ca-app-pub-3940256099942544/8691691433
激励广告	ca-app-pub-3940256099942544/5224354917
插页式激励广告	ca-app-pub-3940256099942544/5354046379
原生高级广告	ca-app-pub-3940256099942544/2247696110
原生高级视频广告	ca-app-pub-3940256099942544/1044960115
```

*iOS广告测试ID*
```
开屏广告	ca-app-pub-3940256099942544/5662855259
横幅广告	ca-app-pub-3940256099942544/2934735716
插页式广告	ca-app-pub-3940256099942544/4411468910
插页式视频广告	ca-app-pub-3940256099942544/5135589807
激励广告	ca-app-pub-3940256099942544/1712485313
插页式激励广告	ca-app-pub-3940256099942544/6978759866
原生高级广告	ca-app-pub-3940256099942544/3986624511
原生高级视频广告	ca-app-pub-3940256099942544/2521693316
```
在测试版中可以使用上面的测试ID来展示测试广告。
```dart
  static String get bannerAdUnitId {
    if (kReleaseMode) {
      return "申请的广告ID";
    }

    return Platform.isAndroid
        ? 'ca-app-pub-3940256099942544/6300978111'
        : 'ca-app-pub-3940256099942544/2934735716';
  }
```


### 

添加广告的初始化。
```dart
    MobileAds.instance.initialize();
    runApp(MyApp());
```

**Banner广告**

示例代码，创建一个Banner广告的Widget。  
```dart
import 'dart:io' show Platform;

import 'package:flutter/material.dart';
import 'package:google_mobile_ads/google_mobile_ads.dart';

import '../util/log_util.dart';

class BannerAdWidget extends StatefulWidget {
  const BannerAdWidget({super.key, this.size, required this.adUnitId});

  final AdSize? size;
  final String adUnitId;

  @override
  State<StatefulWidget> createState() => _BannerAdState();
}

class _BannerAdState extends State<BannerAdWidget> {
  BannerAd? _anchoredBanner;
  bool _loadingAnchoredBanner = false;

  Future<void> _createAnchoredBanner(BuildContext context) async {
    AnchoredAdaptiveBannerAdSize? size;
    if (widget.size == null) {
      size = await AdSize.getCurrentOrientationAnchoredAdaptiveBannerAdSize(
          MediaQuery.of(context).size.width.truncate());

      if (size == null) {
        LogUtil.print('Unable to get height of anchored banner.');
        return;
      }
    }

    final BannerAd banner = BannerAd(
      size: widget.size ?? size!,
      request: const AdRequest(),
      adUnitId: widget.adUnitId,
      listener: BannerAdListener(
        onAdLoaded: (Ad ad) {
          LogUtil.print('BannerAd loaded.');
          setState(() {
            _anchoredBanner = ad as BannerAd?;
          });
        },
        onAdFailedToLoad: (Ad ad, LoadAdError error) {
          LogUtil.print('$BannerAd failedToLoad: $error');
          ad.dispose();
        },
        onAdOpened: (Ad ad) => LogUtil.print('$BannerAd onAdOpened.'),
        onAdClosed: (Ad ad) => LogUtil.print('$BannerAd onAdClosed.'),
      ),
    );
    return banner.load();
  }

  @override
  void initState() {
    super.initState();
  }

  @override
  void dispose() {
    _anchoredBanner?.dispose();
    _anchoredBanner = null;
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    if (Platform.isWindows) {
      return Container();
    }

    if (!_loadingAnchoredBanner) {
      _loadingAnchoredBanner = true;
      _createAnchoredBanner(context);
    }

    if (_anchoredBanner != null) {
      return SizedBox(
        //  color: Colors.black,
        width: _anchoredBanner!.size.width.toDouble(),
        height: _anchoredBanner!.size.height.toDouble(),
        child: AdWidget(ad: _anchoredBanner!),
      );
    }
    return Container();
  }
}
```
使用时调用下面的代码即可。
```dart
BannerAdWidget(
    size: null,
    adUnitId: AdHelper.bannerAdUnitId,
)
```

在debug版中，即可显示测试广告。如果需要显示自己申请的广告，需要设定测试设备。  

在使用上面的广告Widget时，会输出如下的log，可以确定设备的ID。

*Use RequestConfiguration.Builder().setTestDeviceIds(Arrays.asList("1EEF1FD322E51C4F7B856AC100E5D\***")) to get test ads on this device.*

添加该设备ID到代码中。
```dart
if (kDebugMode) {
  MobileAds.instance.updateRequestConfiguration(RequestConfiguration(
      testDeviceIds: ['1EEF1FD322E51C4F7B856AC100E5D***']));
}
runApp(MyApp());
```

添加上面的代码后，运行时log中会显示下面的信息，表示在使用一台测试设备。    

*This request is sent from a test device.*










---

> Author:   
> URL: http://localhost:1313/tech/build-flutter-app8/  

