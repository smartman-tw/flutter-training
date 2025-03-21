# 表單設計與互動性

參考資料
* [官方文件: 輸入與表單](https://docs.flutter.dev/cookbook/forms/text-input)
* [官方文件: 觸擊偵測 (Gestures)](https://docs.flutter.dev/ui/interactivity/gestures)
### 欄位元件
| 元件 | 說明 |
| --- | --- |
| TextField / TextFormField | 文字輸入框 |
| DropdownButton / DropdownButtonFormField | 下拉選單 |
| Checkbox / CheckboxListTile| 勾選框 |
| InkWell | 點擊元件 (有漣漪效果) |
| GestureDetector | 手勢偵測元件 (無漣漪、但可偵測更多手勢互動) |
| Switch | 開關元件 |
### 練習
> 概念: 下拉選單、自訂按鈕、日期選擇器、文字欄位、checkbox

底下為Google Maps上的預約餐廳的表單：

![alt text](Images/inline_demo.gif)
#### 實作要點:
* 三個頁面切換: 選擇日期時段 --> 確認預約資訊 <----> 編輯聯絡人資訊。
* 選擇日期時段頁面: 
  * 下拉選單: 選擇人數、選擇時段。
  * 時段選擇: 也透過下方按鈕選擇。
  * 日期選擇: 透過日期選擇器實作。
* 確認預約資訊頁面:
  * 編輯聯絡人資訊按鈕
  * 特殊要求輸入框
  * 透過Google預定checkbox
* 聯絡人資訊: 姓氏、名字、電話、電子郵件輸入框