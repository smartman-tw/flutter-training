# Distribution
要將自己寫得好的Flutter APP分享給其他人，最快速的方式是執行底下語法:
```bash
flutter build apk --target-platform android-arm64
```
執行完之後，在`{專案名稱}\build\app\outputs\flutter-apk`資料夾中會產生一個`app-release.apk`檔案，這就是可以直接安裝的APK檔案。可以直接把這個檔案寄給別人，別人點選後就會自行安裝。
> 此為Android的APK檔案，iOS的話需要使用Xcode來打包，這邊不做說明。

[參考資料](https://stackoverflow.com/questions/65566952/fastest-way-to-share-a-flutter-app-with-someone-else)