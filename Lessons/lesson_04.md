# 如何排版?
> 水平、垂直、滾動、重疊等排版技巧

更多請參考官方文件 [Flutter Layout](https://docs.flutter.dev/ui/layout)。

| Widget / Keyword | 用途 | 常用參數 |
| --- | --- | --- |
| `Row` | 水平排列 | `mainAxisAlignment` (左右隊齊), `crossAxisAlignment` (上下隊齊), `spacing` (間距) |
| `Column` | 垂直排列 | `mainAxisAlignment` (上下隊齊), `crossAxisAlignment` (左右隊齊), `spacing` (間距)|
| `ListView` | 滾動排列 | `scrollDirection` (滾動方向) |
| `Stack` | 重疊排列 | `alignment` |
| `Wrap` | 自動換行排列 | `alignment` |
| `Expanded` | 自動填滿剩餘空間 | `flex` |
| `SingleChildScrollView` | 單一子元件滾動 | `scrollDirection` (滾動方向) |
| `Spacer` | 自動填滿剩餘空間 |  |
| `MediaQuery.of(context).size` | 取得螢幕尺寸 |  |
## 練習
請試著用上述5種排版技巧，複製以下UberEats首頁的畫面:
>圖片部分可以使用icons或色塊取代

![alt text](images/ubereats_homepage.png)