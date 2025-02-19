# 常用元件、Dart語言

## Dart語言
請參考 [Dart 語言官方文件](https://dart.dev/language)。
- [Dart 不同的建構子](https://dart.dev/language/constructors)
## What's new
- Dart 3.7 以後尾行逗號(trailing comma)與換行排版(formatting)的邏輯有調整。先前是可以在程式碼中透過新增或移除trailling comma決定是否換行，3.7以後變成若元素只有一個時強制無法換行(就算手動補上trailing comma)，一個元素以上儲存後補上trailling comma並會自動換行。
    > [Dart 3.7新排版](https://codewithandrea.com/articles/new-formatting-style-dart-3-7/)
    > 在 pubspec.yaml 可以看到 Dart SDK 的版本。
    ```yaml
    environment:
        sdk: ^3.6.0
    ```
## 常用元件
1. **Text**: 用來顯示文字。
2. **Image**: 用來顯示圖片。
3. **Icon**: 用來顯示icon。
4. **Container**: 用來放其他元件的容器，可以自訂長寬、邊框、顏色等。
5. **SizedBox**: 一個只能設定長寬的容器。
6. **Padding**: 用來設定元件的間距。
7. **Center**: 用來將元件置中。

## 排版元件 (參數都吃 children)
1. **Column**: 垂直排列元件。(主軸為垂直)
2. **Row**: 水平排列元件。(主軸為水平)
3. **Wrap**: 自動換行排列元件。
4. **ListView**: 用來顯示列表的元件。
5. **SingleChildScrollView**: 用來顯示滾動的元件。

完整的元件列表請參考 [Flutter Widget Catalog](https://docs.flutter.dev/ui/widgets)。

## VS Code 套件 (Extensions)
1. Flutter: Flutter 官方提供的擴充功能。
2. Flutter Widget Snippets: 可以快速建立 Stateful/Stateless widget。
