# 狀態管理、畫面切換
> 資料的傳遞、儲存與共用
## Navigator 2.0
請參考 [Navigation Basics](https://docs.flutter.dev/cookbook/navigation/navigation-basics)

### 常用切換方式:
1. 前往一個頁面
    ```dart
    Navigator.of(context).push(
        MaterialPageRoute(builder: (context) => const SecondRoute()),
    );
    ```
2. 返回上一個頁面
    ```dart
    Navigator.of(context).pop();
    ```
3. 前往並移除先前所有頁面
    ```dart
    Navigator.of(context).pushAndRemoveUntil(
        MaterialPageRoute(builder: (context) => const HomeScreen()),
        (route) => false,
    );
    ```
## GoRouter
* 套件連結 [GoRouter](https://pub.dev/packages/go_router)
* 設定 [Get Started](https://pub.dev/documentation/go_router/latest/topics/Get%20started-topic.html)
* 回傳值、傳值 [Navigation topic](https://pub.dev/documentation/go_router/latest/topics/Navigation-topic.html)

## 畫面切換元件
* `BottomNavigationBar`: 底部導覽列 [BottomNavigationBar](https://api.flutter.dev/flutter/material/BottomNavigationBar-class.html?source=post_page---------------------------)
* `TabBar`: 上方Tab [TabBar](https://api.flutter.dev/flutter/material/TabBar-class.html)
* `Drawer`: 左側拉出選單 [Drawer](https://docs.flutter.dev/cookbook/design/drawer)
* `AlertDialog`: 警告視窗 [AlertDialog](https://api.flutter.dev/flutter/material/AlertDialog-class.html)
* `BottomSheet`: 底部彈出視窗 [ShowModalBottomSheet](https://api.flutter.dev/flutter/material/showModalBottomSheet.html)
## Reminder
上機測驗30分鐘，內容跟作業類似。(這次就沒作業) 在有效時間盡可能完成即可，過程可以上網找答案。
