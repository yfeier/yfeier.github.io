# 利用Flutter做成应用5：抽屉菜单



### 抽屉菜单
为了在app的顶部左边添加滑出菜单，需要在Scaffold中添加drawer字段，此时在appbar的左侧会出现一个三道杠的图标，点击后即可弹出自定义的菜单。
```dart
Scaffold(
      drawer: const DrawerWidget(),
);
```

示例代码如下：
```dart
import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart';

import '../data/global.dart';
import '../util/log_util.dart';
import '../util/ui_util.dart';
import 'setting.dart';

class DrawerWidget extends StatelessWidget {
  const DrawerWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: MediaQuery.removePadding(
          context: context,
          removeTop: false,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              Padding(
                padding: const EdgeInsets.only(top: 40.0),
                child: Center(
                  child: ClipOval(
                    child: Image.asset(
                      "assets/imgs/avatar.png",
                      width: 120,
                    ),
                  ),
                ),
              ),
              Expanded(
                  child: ListView(
                children: <Widget>[
                  const Divider(),
                  ListTile(
                    leading: const Icon(Icons.settings),
                    title: const Text(
                      'Setting',
                      textScaleFactor: 1.4,
                    ),
                    trailing: const Icon(Icons.arrow_forward),
                    onTap: () {
                      LogUtil.print('setting');
                      Navigator.push(context,
                          MaterialPageRoute(builder: (BuildContext context) {
                        return const SettingWidget();
                      }));
                    },
                  ),
                  const Divider(),
                  ListTile(
                    leading: const Icon(Icons.info),
                    title: const Text(
                      'About',
                      textScaleFactor: 1.4,
                    ),
                    trailing: const Icon(Icons.arrow_forward),
                    onTap: () {
                      LogUtil.print('about');
                      Navigator.push(context,
                          MaterialPageRoute(builder: (BuildContext context) {
                        return openAbout();
                      }));
                    },
                  ),
                  const Divider(),
                  ListTile(
                      leading: const Icon(Icons.star),
                      title: const Text(
                        'Rate on Google Play',
                        textScaleFactor: 1.3,
                      ),
                      trailing: const Icon(Icons.arrow_forward),
                      onTap: () {
                        LogUtil.print('Rate');
                        _launchUrl(Global.googlePlayUrl);
                      }),
                  const Divider(),
                ],
              ))
            ],
          )),
    );
  }

}

```

对于菜单项，点击后跳转到其他页面，可使用如下代码：
```dart
onTap: () {
    Navigator.push(
        context,
        MaterialPageRoute(
            builder: (BuildContext context) => const SettingWidget()
        )
    );
},
```

通过上面的方式跳转的页面在左上角会自动生成返回按钮，也可用代码直接返回。
```dart
Navigator.pop(context);
```


---

> Author:   
> URL: http://localhost:1313/tech/build-flutter-app5/  

