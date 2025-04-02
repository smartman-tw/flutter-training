# Distribution
## 快速發行
要將自己寫得好的Flutter APP分享給其他人，最快速的方式是執行底下語法:
```bash
flutter build apk --target-platform android-arm64
```
執行完之後，在`{專案名稱}\build\app\outputs\flutter-apk`資料夾中會產生一個`app-release.apk`檔案，這就是可以直接安裝的APK檔案。可以直接把這個檔案寄給別人，別人點選後就會自行安裝。
> 此為Android的APK檔案，iOS的話需要使用Xcode來打包，這邊不做說明。

[快速發行的參考資料](https://stackoverflow.com/questions/65566952/fastest-way-to-share-a-flutter-app-with-someone-else)

## 發行到商城
完整的發行到商城的步驟較多，這邊不做說明。可以參考 [Deployment Android](https://docs.flutter.dev/deployment/android) 與 [Deployment iOS](https://docs.flutter.dev/deployment/ios)。
## 自訂專案名稱、圖示與啟動畫面
* 可以參考 [How to change app name](https://stackoverflow.com/questions/49353199/how-can-i-change-the-app-display-name-build-with-flutter) 去改變專案的名稱
* [flutter_launcher_icons](https://pub.dev/packages/flutter_launcher_icons) 可以將專案的圖示改成自己的圖示
* [flutter_native_splash](https://pub.dev/packages/flutter_native_splash) 可以將專案的啟動畫面改成自己的圖片
