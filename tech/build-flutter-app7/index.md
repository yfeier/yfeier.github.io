# 利用Flutter做成应用7：音乐播放



### 音乐播放

为了在app中播放音乐，可使用插件[audioplayers](https://pub.flutter-io.cn/packages/audioplayers)。 


示例代码如下：
```dart
import 'package:audioplayers/audioplayers.dart';
import 'package:sp_util/sp_util.dart';

import '../data/global.dart';
import 'log_util.dart';

class SEUtil {
  static AudioPlayer player = AudioPlayer();

  static play(String fileName) async {
    try {
      await player.setSourceAsset(fileName);
      await player.setReleaseMode(ReleaseMode.loop);
      await player.resume();

      player.onPlayerStateChanged.listen((event) {
        if (player.state == PlayerState.playing) {
          LogUtil.print('play start');
          Global.bMusic = true;
        } else {
          LogUtil.print('play stop');
          Global.bMusic = false;
        }

        SpUtil.putBool(Global.keyMusic, Global.bMusic!);
      });
    } catch (e) {
      LogUtil.print(e);
    }
  }

  static stop() async {
    await player.stop();
  }

  static release() async {
    LogUtil.print('play release');
    await player.release();
  }
}

```

在yaml文件中配置需要播放的音乐资源，资源应放在assets文件夹下。
```yaml
flutter:
    assets:
        - assets/sounds/moon.mp3
```

即可播放该mp3文件。
```dart
SEUtil.play('sounds/moon.mp3');
```

---

> Author:   
> URL: http://localhost:1313/tech/build-flutter-app7/  

