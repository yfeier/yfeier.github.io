# 利用Flutter做成应用6：主题，字体与资源



### 主题

设置主题的不同颜色，可通过插件[provider](https://pub.flutter-io.cn/packages/provider)来实现对主题颜色的状态变化的管理。


先实现一个类继承ChangeNotifier。
```dart
class ThemeProvider with ChangeNotifier {
  String _themeColor = '';

  String get themeColor => _themeColor;

  setTheme(String themeColor) {
    _themeColor = themeColor;

    notifyListeners();
  }
}
```

在主程序中，通过在MultiProvider中引入ThemeProvider，再通过Consumer来监听主题颜色变量。
颜色变量变化后主题颜色即可实时反映到AppBar上。

```dart
  Widget build(BuildContext context) {
    return MultiProvider(
        providers: [
          ChangeNotifierProvider(create: (_) => ThemeProvider()),
        ],
        child: Consumer<ThemeProvider>(
          builder: (context, themeInfo, _) {
            String colorKey = themeInfo.themeColor;
            if (themeColorMap[colorKey] != null) {
              themeColor = themeColorMap[colorKey];
            }

            return MaterialApp(
              debugShowCheckedModeBanner: false,
              title: Global.appTitle,
              theme: ThemeData(
                primarySwatch: themeColor,
                visualDensity: VisualDensity.adaptivePlatformDensity,
              ),
              home: const HomeRoute(),
            );
          },
        ));
  }
```

设置变量时可以使用下面的方式。
```dart
Provider.of<ThemeProvider>(context, listen: false).setTheme(key);
```
或
```dart
context.read<ThemeProvider>().setTheme(key);
```

### 深色模式
设定app的深色模式，控件可使用插件[day_night_switcher](https://pub.flutter-io.cn/packages/day_night_switcher)中的DayNightSwitcher。

```yaml
dependencies:
  day_night_switcher: ^0.2.0+1
```

控件使用如下，可以控制一个变量来表示是否是深色模式。
```dart
DayNightSwitcher(
    isDarkModeEnabled: isDarkModeEnabled,
    onStateChanged: (isDarkMode) {
      setState(() {
        isDarkModeEnabled = isDarkMode;
        LogUtil.print("isDarkMode: $isDarkMode");
      });
      context.read<ThemeProvider>().setDarkMode(isDarkMode);
    },
  ),
```

在MaterialApp设置深色模式。
```dart
darkTheme: ThemeData.dark().copyWith(
  appBarTheme: const AppBarTheme(color: Color(0xFF253341)),
  scaffoldBackgroundColor: const Color(0xFF15202B),
),
themeMode: bDarkMode ? ThemeMode.dark : ThemeMode.light,
```
在这里监控上面的控件设置的变量，即可通过控件来设置深色模式。


### 字体与资源

可以使用[google_fonts](https://pub.flutter-io.cn/packages/google_fonts)来设置文本所使用的字体。
```yaml
dependencies:
  google_fonts: ^3.0.1
```

使用时如下所示：
```dart
Text(
  "test",
  style: GoogleFonts.pacifico(
    textStyle: const TextStyle(fontSize: 24.0),
  ),
),
```


###
也可从[网站](https://fonts.google.com/)预先下载字体。将下载的字体文件放在assets\fonts中。  

在yaml配置文件中配置
```yaml
flutter:
  fonts:
     - family: PassionsConflict
       fonts:
         - asset: assets/fonts/PassionsConflict-Regular.ttf
```

使用时
```dart
Text(
    "test",
    style: const TextStyle(
        fontFamily: 'PassionsConflict',
        fontSize: 24.0,
        fontWeight: FontWeight.bold),
),
```

还应该添加许可文件
```dart
  LicenseRegistry.addLicense(() async* {
    final license = await rootBundle.loadString('assets/fonts/OFL.txt');
    yield LicenseEntryWithLineBreaks(['google_fonts'], license);
  });

  runApp(MyApp());
```

---

> Author:   
> URL: https://yfeier.github.io/tech/build-flutter-app6/  

