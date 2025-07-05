# 利用Flutter做成应用4：存储



### 键值数据存储

对于简单的数据存储，可以使用插件[sp_util](https://pub.flutter-io.cn/packages/sp_util)方便的读写数据，该插件是对[shared_preferences](https://pub.flutter-io.cn/packages/shared_preferences)的实用包装。

```yaml
dependencies:
  sp_util: ^2.0.3
```

使用之前，需要对sp_util进行异步初始化。
```dart
await SpUtil.getInstance();
```

### 复杂数据存储

1) 对于复杂的数据，可以将数据存入到数据库中。可使用插件[sqflite](https://pub.flutter-io.cn/packages/sqflite)进行数据读写。
```yaml
dependencies:
  sqflite: ^2.1.0
```

创建一个工厂类，实现对db的操作。
```dart
import 'package:path/path.dart' as p;
import 'package:sqflite/sqflite.dart';
import 'package:synchronized/synchronized.dart';

const String tblName = "Img";
class SaveUtil {
  // 私有构造函数.
  SaveUtil._();
  // 单例.
  static final SaveUtil _singleton = SaveUtil._();
  // 工厂构造函数.
  factory SaveUtil() => _singleton;

  static final Lock _lock = Lock();

  static Database? _database;
  Future<Database> get database async {
    if (_database == null) {
      await _lock.synchronized(() async {
        _database ??= await _initDb();
      });
    }
    return _database!;
  }

  _initDb() async {
    try {
      String databasesPath = await getDatabasesPath();
      String path = p.join(databasesPath, 'beautyBook.db');

//    await deleteDatabase(path);

      return await openDatabase(path, version: 2, onCreate: _onCreate);
    } catch (e) {
    }
  }

  void _onCreate(Database db, int newVersion) async {
    await db
        .execute('CREATE TABLE $tblName (id INTEGER PRIMARY KEY, url TEXT)');
  }

  Future<int?> save(String url) async {
    final db = await database;
    var result =
        await db.rawInsert("INSERT INTO $tblName(url) VALUES(\"$url\")");
    return result;
  }

  Future<List<String>> get() async {
    List<String> list = [];
    final db = await database;

    List listTmp = await db.rawQuery('SELECT url FROM $tblName');
    for (var map in listTmp) {
      String url = map['url'];
      if (!list.contains(url)) {
        list.add(url);
      }
    }

    return list;
  }

  Future<int?> count() async {
    final db = await database;
    int? count = Sqflite.firstIntValue(
        await db.rawQuery('SELECT COUNT(*) FROM $tblName'));

    return count;
  }

  Future delete(String url) async {
    final db = await database;
    await db.rawDelete('DELETE FROM $tblName WHERE url = ?', [url]);
  }

  Future closeDB() async {
    return _database?.close();
  }
}

```


sqflite不支持windows，在windows平台下，会报下面的错误。
```
MissingPluginException(No implementation found for method getDatabasesPath on channel com.tekartik.sqflite)
```

###
2) 为了支持windows平台，可使用插件[sqflite_common_ffi](https://pub.flutter-io.cn/packages/sqflite_common_ffi)。
```yaml
dependencies:
  sqflite_common_ffi: ^2.1.1+1
```

在iOS和Android上，还需要添加插件[sqlite3_flutter_libs](https://pub.flutter-io.cn/packages/sqlite3_flutter_libs)。
```yaml
dependencies:
  sqlite3_flutter_libs: ^0.5.10
```

否则在手机环境下会出现下面的错误。
```cmd
SqfliteFfiException(error, Invalid argument(s): Failed to load dynamic library '/data/data/com.***/lib/libsqlite3.so'
```

创建如下示例代码。
```dart
import 'dart:io';

import 'package:path_provider/path_provider.dart';
import 'package:sqflite_common/sqlite_api.dart';
import 'package:sqflite_common_ffi/sqflite_ffi.dart';
import 'package:synchronized/synchronized.dart';
import 'package:path/path.dart' as p;

const String tblName = "Img";

class DBUtil {
// 私有构造函数.
  DBUtil._();
  // 单例.
  static final DBUtil _singleton = DBUtil._();
  // 工厂构造函数.
  factory DBUtil() => _singleton;

  static final Lock _lock = Lock();

  static Database? _database;
  Future<Database?> get database async {
    if (_database == null) {
      await _lock.synchronized(() async {
        _database ??= await _initDb();
      });
    }
    return _database;
  }

  _initDb() async {
    try {
      if (Platform.isWindows) {
        sqfliteFfiInit();
      }

      var databaseFactory = databaseFactoryFfi;

      Directory appDocDir = await getApplicationDocumentsDirectory();
      String databasesPath = appDocDir.path;
      String dbPath = p.join(databasesPath, 'beautyOfBook.db');


      var db = await databaseFactory.openDatabase(dbPath,
          options: OpenDatabaseOptions(
              version: 1,
              onCreate: (db, version) async {
                await db.execute(
                    'CREATE TABLE $tblName (id INTEGER PRIMARY KEY, url TEXT)');
              }));

      return db;
    } catch (e) {
    }
  }

  Future<int?> save(String url) async {
    final db = await database;
    if (db == null) {
      return 0;
    }
    //var result =
    //    await db.rawInsert("INSERT INTO $tblName(url) VALUES(\"$url\")");
    var result = await db.insert(tblName, {'url': url});
    return result;
  }

  Future<List<String>> get() async {
    final db = await database;
    if (db == null) {
      return [];
    }

    db.query(tblName, distinct: true, columns: ['url']);
    List<String> list = [];
    //List listTmp = await db.rawQuery('SELECT url FROM $tblName');
    List listTmp = await db.query(tblName, distinct: true, columns: ['url']);
    for (var map in listTmp) {
      String url = map['url'];
      if (!list.contains(url)) {
        list.add(url);
      }
    }

    return list;
  }

  Future<int?> count() async {
    final db = await database;
    if (db == null) {
      return 0;
    }

    //int? count =
    //    firstIntValue(await db.rawQuery('SELECT COUNT(*) FROM $tblName'));
    final results =  await db.query('SELECT COUNT(*) FROM $tblName');
    int? count = results.first.values.first as int?;

    return count;
  }

  Future delete(String url) async {
    final db = await database;
    if (db == null) {
      LogUtil.print("db is null when delete");
      return 0;
    }
    //await db.rawDelete('DELETE FROM $tblName WHERE url = ?', [url]);
    await db.delete(tblName, where: 'url = ?', whereArgs: [url]);
  }

  Future closeDB() async {
    LogUtil.print("closeDB");
    return _database?.close();
  }


}

```

说明：
该插件中的getDatabasesPath()实现不是很好，可以使用插件[path_provider](https://pub.flutter-io.cn/packages/path_provider)来获取db的存储文件夹。
```yaml
dependencies:
  path_provider: ^2.0.11
```
```dart
Directory appDocDir = await getApplicationDocumentsDirectory();  
String databasesPath = appDocDir.path;
```
在android环境下，路径为/data/user/0/[app id]/app_flutter.  
在windows环境下，路径为C:\Users\\[user name]\Documents.  

---

> Author:   
> URL: http://localhost:1313/tech/build-flutter-app4/  

