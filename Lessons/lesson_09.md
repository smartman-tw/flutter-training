# Persistence
> 持久化資料的意思是將資料儲存到磁碟上，這樣即使應用程式關閉或重新啟動，資料仍然可以保留。
 
[官網說明檔案操作](https://docs.flutter.dev/cookbook/persistence/reading-writing-files)

### 檔案操作範例

**將檔案寫入到應用程式的路徑 (documents directory)**
* [path_provider](https://pub.dev/packages/path_provider): 可以取得應用程式的路徑資訊
  * Temporary directory: 應用程式的暫存資料夾。作為**快取**用。系統可以執行清除。
  * Documents directory: 只有應用程式可以讀取的資料夾。應用程式刪除後會被刪除。  
```dart
import 'package:path_provider/path_provider.dart';
import 'dart:io';
//...省略程式碼
final directory = await getApplicationDocumentsDirectory();
final file = File('${directory.path}/example.txt');
await file.writeAsString('Hello, world!');
```
**開啟檔案**
* [open_file](https://pub.dev/packages/open_file): 可以立即開啟一個指定的檔案
```dart
import 'package:flutter/open_file.dart';
//...省略程式碼
await OpenFile.open(file.path);
```
**挑選與讀取檔案**
* [file_picker](https://pub.dev/packages/file_picker): 可以開啟檔案選擇視窗讓使用者挑選一致多個檔案
```dart
import 'package:file_picker/file_picker.dart';
//...省略程式碼
final result = await FilePicker.platform.pickFiles(); // 挑選檔案
if (result == null) return; // 使用者取消挑選檔案
final file = File(result.files.single.path!);
// 讀取檔案
final data = await file.readAsString();
```
### Shared Preferences
[shared_preferences](https://pub.dev/packages/shared_preferences): 可以儲存一些簡單的資料 (key-value pair)，這些資料會被儲存在應用程式的文件目錄中，並且在應用程式重新啟動後仍然可以存取。
[官網SharedPreferences的說明](https://docs.flutter.dev/cookbook/persistence/key-value)

**將陣列儲存到Shared Preferences**
```dart
import 'package:shared_preferences/shared_preferences.dart';
class Todo {
  final String name;
  final bool isDone;
  // 建構子
  Todo({required this.name, this.isDone = false});
  // 轉成Map
  Map<String, dynamic> toJson() {
    return {
      'name': name,
      'isDone': isDone,
    };
  }
}
//...省略程式碼
void saveTodo(List<Todo> todoItems) {
    SharedPreferences.getInstance().then((prefs) {
        // 將Todo物件陣列序列化成字串並存到Shared Preferences
        prefs.setString(
            'todo', json.encode(todoItems.map((e) => e.toJson()).toList()));
    });
}
```
> 所謂序列化，就是將物件轉成字串的動作。對Dart來說就是把原生型態(int、double、bool、String、List、Map)轉成字串。我們要用的是`json.encode()`，這個方法會將物件轉成字串。所以我們才把Todo物件一個一個先轉成Map物件 (`toJson`)，然後再將這些Map物件轉成List物件 (`toList`)，最後再將List物件轉成字串 (`json.encode`)。

**從Shared Preferences讀取陣列**
```dart
SharedPreferences.getInstance().then((prefs) {
    final todoString = prefs.getString('todo');
    if (todoString != null) {
    final List<dynamic> todoList = json.decode(todoString); // 將字串反序列化成List
    var todoItems = todoList
        .map((e) => Todo(name: e['name'], isDone: e['isDone']))
        .toList();
    }
});
```
> 反之，json.decode()則是將字串轉成物件。json.decode()可以將字串轉成Map物件或List物件，這取決於字串的內容。如果字串是以`[`開頭的，那麼它會被轉成List物件；如果字串是以`{`開頭的，那麼它會被轉成Map物件。每個物件都是