# Device Features
在Flutter中使用裝置的功能 (相機、相簿、定位、通知) 都是透過套件來實現的，這些套件通常會提供一些API來讓開發者使用這些功能。由於部分的個人專題有用到其他裝置功能如語音轉文字、掃碼與辨識的功能。底下會在底部附上這些功能的參考用套件。

> [!NOTE]
> 通常使用到裝置功能的套件都會需要針對Android或iOS進行一些設定，這些設定通常會在套件的文件中說明。例如需要在AndroidManifest.xml中加入權限或是服務的設定。
## Table of Contents
- [Device Features](#device-features)
  - [Table of Contents](#table-of-contents)
  - [選擇圖片](#選擇圖片)
  - [通知功能](#通知功能)
    - [Android 設定](#android-設定)
    - [通知功能範例](#通知功能範例)
  - [取得位置](#取得位置)
  - [取得使用者的授權狀態](#取得使用者的授權狀態)
  - [語音轉文字](#語音轉文字)
  - [掃碼與辨識](#掃碼與辨識)

## 選擇圖片 
[image_picker](https://pub.dev/packages/image_picker) 可以選擇圖片或影片，並且可以從相機或相簿中選擇。這個套件也可以拍攝照片或錄製影片。其中圖片來源可以選擇相機(camera)或是相簿(gallery)，在`source`參數中指定。
```dart
import 'package:image_picker/image_picker.dart';
// 選擇圖片的路徑
String? _selectedImagePath;
// 選擇圖片
Future<void> selectImage() async {
    final ImagePicker picker = ImagePicker();
    final XFile? image = await picker.pickImage(source: ImageSource.gallery);
    if (image != null) {
        print('選擇的圖片路徑: ${image.path}');
        setState(() {
            _selectedImagePath = image.path;
        });
    } else {
        print('沒有選擇圖片');
    }
}
```
顯示已選擇的圖片
```dart
if (_selectedImagePath != null)
    Image.file(
        File(_selectedImagePath!),
        height: 300,
    ),
```
## 通知功能 

[flutter_local_notifications](https://pub.dev/packages/flutter_local_notifications) 可以在手機中發送簡易的推播通知，也可以設定排程通知 (scheduled notifications)，例如每天的某個時間發送通知。同時，也可以監聽通知的點擊事件，當使用者點擊通知時，可以執行一些操作。

### Android 設定
通知功能需要比較多的設定，但不用擔心，只要遵循套件的文件步驟一步一步走就可以完成。[Android的設定](https://pub.dev/packages/flutter_local_notifications#-android-setup)。(而且也不用每一步都做，只要看到說明有說 if... 或是 for apps that...，代表這個步驟是選擇性的，或是有些步驟是針對特定的功能才需要做的。) 指下列出必要步驟:
1. 修改 `build.gradle.kts` 檔案:
    > 請善用 VS Code 搜尋檔案名稱的功能，按下 `Ctrl + P`，然後輸入 `build.gradle.kts`，就可以快速找到並開啟檔案。
    ```kotlin
    android {
        defaultConfig {
            multiDexEnabled = true
        }

        compileOptions {
            // Flag to enable support for the new language APIs
            isCoreLibraryDesugaringEnabled = true
            // Sets Java compatibility to Java 11
            sourceCompatibility = JavaVersion.VERSION_11
            targetCompatibility = JavaVersion.VERSION_11
        }
    
        kotlinOptions {
            jvmTarget = JavaVersion.VERSION_11.toString()
        }
    }

    dependencies {
        coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.4")
    }
    ```
2. 針對排程通知的功能，還需要在 `AndroidManifest.xml` 中的 `<application>` 標籤中加入以下的權限與服務設定:
    ```xml
    <receiver android:exported="false" android:name="com.dexterous.flutterlocalnotifications.ScheduledNotificationReceiver" />
    <receiver android:exported="false" android:name="com.dexterous.flutterlocalnotifications.ScheduledNotificationBootReceiver">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED"/>
            <action android:name="android.intent.action.MY_PACKAGE_REPLACED"/>
            <action android:name="android.intent.action.QUICKBOOT_POWERON" />
            <action android:name="com.htc.intent.action.QUICKBOOT_POWERON"/>
        </intent-filter>
    </receiver>

    ```
### 通知功能範例
要看範例除了可以在 pub.dev 上了套件 Example 以外，最完整的範例通常會在套件的 GitHub 中。完整範例請參考 [flutter_local_notifications/example](https://github.com/MaikuB/flutter_local_notifications/blob/master/flutter_local_notifications/example/lib/main.dart)。



## 取得位置 
[geolocator](https://pub.dev/packages/geolocator) 可以取得裝置的當前位置，並且可以監聽位置的變化。這個套件也可以取得使用者的授權狀態 (permission status)，例如是否允許使用者使用定位功能。
```dart
Future<Position> _determinePosition() async {
    bool serviceEnabled;
    LocationPermission permission;

    // 檢查位置服務是否啟用
    serviceEnabled = await Geolocator.isLocationServiceEnabled();
    if (!serviceEnabled) {
        return Future.error('服務沒有被啟用');
    }

    permission = await Geolocator.checkPermission();
    if (permission == LocationPermission.denied) {
        permission = await Geolocator.requestPermission();
        if (permission == LocationPermission.denied) {
            return Future.error('位置權限被拒絕');
        }
    }

    if (permission == LocationPermission.deniedForever) {
        return Future.error('位置權限被永久拒絕，請在設定中啟用');
    }
    // 取得當前位置
    return await Geolocator.getCurrentPosition();
}
```
> [!TIP] 
> 如同文件中的範例所示，建議使用提早回傳 (early return) 以減少巢狀結構的深度，讓程式碼更容易閱讀。

## 取得使用者的授權狀態 
[permission_handler](https://pub.dev/packages/permission_handler) 可以抓取手機所有不同的權限狀態，並且可以請求使用者授權。由於使用方式簡單在此不贅述。

## 語音轉文字 
[speech_to_text](https://pub.dev/packages/speech_to_text) 可以將使用者的語音轉換成文字，並且可以辨識語音的內容。這個套件也可以監聽語音的變化，例如當使用者說話時，可以即時顯示語音的內容。

## 掃碼與辨識 
[mobile_scanner](https://pub.dev/packages/mobile_scanner) 可以使用手機的相機掃描條碼或QR碼，並且可以辨識條碼或QR碼的內容。


