# 利用Flutter做成应用2：APP做成


### 通过android studio创建应用  
1. 选择 File>New Flutter Project
2. 选择 Flutter application 作为 project 类型, 然后点击 Next
3. 输入项目名称 (如 myapp), 然后点击 Next
4. 点击 Finish
5. 等待Android Studio安装SDK并创建项目.

使用如下代码，做成一个空的app，该app有一个AppBar，底部有四个按钮。  

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Beauty of Book',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: const HomeRoute(),
    );
  }
}

class HomeRoute extends StatefulWidget {
  const HomeRoute({super.key});

  @override
  State<StatefulWidget> createState() => _HomeRouteState();
}

class _HomeRouteState extends State<HomeRoute> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Beauty of Book'),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.photo), label: 'Home'),
          BottomNavigationBarItem(
              icon: Icon(Icons.collections), label: 'History'),
          BottomNavigationBarItem(
              icon: Icon(Icons.pregnant_woman), label: 'Cosplay'),
          BottomNavigationBarItem(
              icon: Icon(Icons.favorite), label: 'Favorite'),
        ],
        fixedColor: Colors.teal,
        type: BottomNavigationBarType.fixed,
      ),
    );
  }
}

```


### 现在需要对底部的四个按钮实现不同的内容。

实现BottomNavigationBar中的currentIndex和onTap属性，即可在不同的Tab之间进行切换。

```dart
bottomNavigationBar: BottomNavigationBar(
        currentIndex: _currentTabIndex,
        onTap: _onItemTapped,
      ),

void _onItemTapped(int index) {
    if (_currentTabIndex != index) {
        setState(() {
            _currentTabIndex = index;
        });
    }
}

```


不同tab对应的页面内容，使用PageView进行管理。  
在Scaffold的body属性中，添加PageView组件。为了防止手机刘海等对内容的阻挡，可以用先用SafeArea进行包裹。 
```dart
final _pageController = PageController(initialPage: 0);
``` 
```dart
  void _pageChange(int index) {
    setState(() {
      if (_currentTabIndex != index) {
        _currentTabIndex = index;
      }
    });
  }
```
```dart
      body: SafeArea(
        child: Stack(
          alignment: AlignmentDirectional.bottomCenter,
          children: [
            PageView(
              onPageChanged: _pageChange,
              controller: _pageController,
              children: const [
                TabWidget(index: TabType.home),
                TabWidget(index: TabType.history),
                TabWidget(index: TabType.cosplay),
                TabWidget(index: TabType.favorite)
              ],
            )
          ],
        ),
      ),
```

在_onItemTapped中添加页面跳转控制。
```
_pageController.jumpToPage(index);
```

对于PageView中children指定的不同页面，可以新建一个文件来实现该页面内容。
```dart
import 'package:flutter/material.dart';

enum TabType { home, history, cosplay, favorite }

class TabWidget extends StatefulWidget {
  const TabWidget({super.key, required this.index});
  final TabType index;

  @override
  State<StatefulWidget> createState() => _TabWidgetState();
}

class _TabWidgetState extends State<TabWidget> {
  @override
  Widget build(BuildContext context) {
    Widget tabWidget;
    switch (widget.index) {
      case TabType.home:
        tabWidget = const Text("home");
        break;
      case TabType.history:
        tabWidget = const Text("history");
        break;
      case TabType.cosplay:
        tabWidget = const Text("cosplay");
        break;
      case TabType.favorite:
        tabWidget = const Text("favorite");
        break;
    }

    return tabWidget;
  }

```

至此，点击底部不同的tab图标即可显示不同的页面内容。


#### Tips
1) 每次切换tab时，前一个tab的内容就会被销毁，导致切换回前一个tab时，tab的内容又要重新加载。
为了保持前一个页面的内容，添加AutomaticKeepAliveClientMixin。

```dart
class _TabWidgetState extends State<TabWidget> with AutomaticKeepAliveClientMixin 
```

并实现方法wantKeepAlive, 需要保持页面内容的返回true即可。

```dart
@override
bool get wantKeepAlive => true;
```

2) 为了避免用户误触返回按钮而导致App退出，可以使用WillPopScope拦截用户点击返回的操作。在用户点击返回时，弹出退出确认框。确认框使用插件[adaptive_dialog](https://pub.flutter-io.cn/packages/adaptive_dialog)实现。

代码如下：
```dart
  WillPopScope(
    onWillPop: () async {
      OkCancelResult bRet = await showOkCancelAlertDialog(
        context: context,
        message: 'Would you like to exit now?',
        okLabel: 'DECIDE',
        cancelLabel: 'CANCEL',
      );
      if (bRet == OkCancelResult.ok) {
        return true;
      }
      return false;
    },
    child: Scaffold(
    ),
  );
```

---

> Author:   
> URL: https://yfeier.github.io/tech/build-flutter-app2/  

