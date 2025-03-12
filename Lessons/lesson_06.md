
# Riverpod

## 安裝
在 Terminal 輸入以下指令
```bash
flutter pub add flutter_riverpod
```
## 概念
* Provider: 狀態的提供者
* Consumer: 狀態的使用者(消費者)，當狀態改變時會重新建構

## 使用 Riverpod
### 事前準備
* 在 `main.dart` 中加入 `ProviderScope`
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
void main() {
    runApp(ProviderScope(child: MyApp()));
}
```
### 建立一個 Provider
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
// 餐點資料 Provider
final mealsProvider = Provider((ref) {
    return //...省略程式碼
});
```
### 使用 Provider
* 將原本 `StatefulWidget` 改為 `ConsumerStatefulWidget`、或是 `StatelessWidget` 改為 `ConsumerWidget`。
    > 這個動作可以透過 VS Code 的 Flutter Riverpod Snippet extension 快速完成。按下 `Ctrl + .` 就可以自動轉換。
* 使用 `ref.watch` 來監聽 Provider 的狀態。
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
class TabScreen extends ConsumerStatefulWidget {
    const TabScreen({super.key});

    @override
    _TabScreenState createState() => _TabScreenState();
}

class _TabScreenState extends ConsumerState<TabScreen> {
    @override
    Widget build(BuildContext context) {
        // 取得餐點清單
        final meals = ref.watch(mealsProvider);
        // ...省略程式碼
    }
}
```
### 改變 Provider 的狀態
定義 `StateNotifier` 類別，並透過 `StateNotifierProvider` 來建立一個可以改變狀態的 Provider。

* 建立一個類別並 extends `StateNotifier`。`StateNotifier` 是一個泛型 (generic) 類別，可以在`<>`中指定要資料型態。`super` 則是呼叫父類別的建構子。以下的例子中，`super([])` 是呼叫父類別的建構子，並傳入一個空的 List 來初始化空的餐點清單。
* 改變狀態的是清除舊的資料，並重新建立新的資料。而不是直接修改資料。要更改資料，可以取得 `state` 並透過 `state =` 來改變資料。
* 最後透過 `StateNotifierProvider` 來建立一個可以改變狀態的 Provider。跟 Provider 類似，`StateNotifierProvider` 接受一個方法，並回傳一個 `StateNotifier` 的實例。同時 `StateNotifierProvider` 也是一個泛型類別，可以在 `<>` 中指定要資料型態。但需要指定兩個型態，第一個是 `StateNotifier` 的型態，第二個是 `StateNotifier` 的 `state` 的型態。
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:meal_app/models/meal.dart';
class FavoriteMealsNotifier extends StateNotifier<List<Meal>> {
    FavoriteMealsNotifier() : super([]);
    // 切換餐點的最愛狀態
    void toggleMealFavoriteStatus(Meal meal) {
        final mealIsFavorite = state.contains(meal);
        if (mealIsFavorite) {
            state = state.where((m) => m.id != m.id).toList();
        } else {
            state = [...state, meal];
        }
    }
}

final favoriteMealsProvider = StateNotifierProvider<FavoriteMealsNotifier, List<Meal>>((ref) {
    return FavoriteMealsNotifier();
});
```
> `...` 是展開運算子 (Spread operator)，可以將 List 展開成多個元素。如 `[...[1, 2], 3]` 會變成 `[1, 2, 3]`。

* 使用 `ref.read` 來取得 Provider 的實例，並透過 `notifier` 來呼叫 Provider 中的方法。
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

class MealDetailsScreen extends ConsumerWidget {
    final Meal meal;
    const MealDetailsScreen({super.key, required this.meal});

    @override
    Widget build(BuildContext context, WidgetRef ref) {
        return Scaffold(
            // ...省略程式碼
            body: Column(
                children: [
                    Image.network(meal.imageUrl),
                    // ...省略程式碼
                    IconButton(
                        icon: Icon(Icons.favorite),
                        onPressed: () {
                            ref.read(favoriteMealsProvider.notifier).toggleMealFavoriteStatus(meal);
                        },
                    ),
                ],
            ),
        );
    }
}
```
### 利用 Provider 的資料初始化 Widget
以下定義一個飲食篩選的 Provider:
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
// 飲食篩選enum: 無麩質、無乳製品、素食、純素
enum Filter { GlutenFree, LactoseFree, Vegetarian, Vegan }

class FiltersNotifier extends StateNotifier<Map<Filter, bool>> {
    FiltersNotifier() : super({
        Filter.GlutenFree: false,
        Filter.LactoseFree: false,
        Filter.Vegetarian: false,
        Filter.Vegan: false,
    });
    // 設定飲食篩選
    void setFilter(Filter filter, bool isActive) {
        state = {...state, filter: isActive};
    }
    void setFilters(Map<Filter, bool> filters) {
        state = filters;
    }
}

final filtersProvider = StateNotifierProvider<FiltersNotifie, Map<Filter, bool>>((ref) => FiltersNotifier());
```
> * 留意這裡不能夠 `state[filter] = isActive` 來改變狀態。
> * `Map` 是一個 key-value 的資料結構，可以透過 key 來取得 value。在這個例子中，`Filter` 是 key 的型態，`bool` 是 value 的型態。

底下在 `initState` 中取得 Provider 的狀態 (透過`ref.read`)，並在 `WillPopScope` 中將飲食篩選的狀態儲存。
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
class FiltersScreen extends ConsumerStatefulWidget {
    const FiltersScreen({super.key});

    @override
    _FiltersScreenState createState() => _FiltersScreenState();
}

class _FiltersScreenState extends ConsumerState<FiltersScreen> {
    // 四個飲食篩選去改變UI
    var _glutenFree = false;
    var _lactoseFree = false;
    var _vegetarian = false;
    var _vegan = false;
    @override
    void initState() {
        super.initState();
        // 取得目前的飲食篩選
        final activeFilters = ref.read(filtersProvider);
        _glutenFree = activeFilters[Filter.GlutenFree];
        _lactoseFree = activeFilters[Filter.LactoseFree];
        _vegetarian = activeFilters[Filter.Vegetarian];
        _vegan = activeFilters[Filter.Vegan];
    }
    @override
    Widget build(BuildContext context) {
        final filters = ref.watch(filtersProvider);
        return Scaffold(
            // ...省略程式碼
            body: WillPopScope(
                onWillPop: () async {
                    ref.read(filtersProvider.notifier).setFilters({
                        Filter.GlutenFree: _glutenFree,
                        Filter.LactoseFree: _lactoseFree,
                        Filter.Vegetarian: _vegetarian,
                        Filter.Vegan: _vegan,
                    });
                    return true;
                },
                child: Column(
                    children: [
                        // ...省略程式碼
                    ],
                ),
            ),
        );
    }
}
```
> 留意在 `initState` 中只能使用 `ref.read` 來取得 Provider 的狀態，而不能使用 `ref.watch`。

### 將狀態的權責都交給 Provider
將狀態的權責都交給 Provider 可以讓 Widget 只需要負責 UI 的呈現而不需要管理狀態。
* 改使用 `ConsumerStatefulWidget` 來取得 Provider 的狀態。
* 使用 `ref.watch` 來監聽 Provider 的狀態。
```dart
class FiltersScreen extends ConsumerStatefulWidget {
    const FiltersScreen({super.key});

    @override
    _FiltersScreenState createState() => _FiltersScreenState();
}

class _FiltersScreenState extends ConsumerState<FiltersScreen> {
    @override
    Widget build(BuildContext context) {
        return Scaffold(
            // ...省略程式碼
            body: WillPopScope(
                // ...省略程式碼
                child: Column(
                    children: [
                        SwitchListTile(
                            title: Text('無麩質'),
                            subtitle: Text('只顯示無麩質食物'),
                            // 監聽篩選狀態
                            value: ref.watch(filtersProvider)[Filter.GlutenFree],
                            onChanged: (newValue) {
                                // 改變單一飲食篩選狀態
                                ref.read(filtersProvider.notifier).setFilter(Filter.GlutenFree, newValue);
                            },
                        ),
                        // ...省略程式碼
                    ],
                ),
            ),
        );
    }
}
```

### 當一個 Provider 需要 (依賴) 另一個 Provider 的狀態
可以透過 `ref` 來取得其他 Provider 的狀態。
```dart
// 過濾後的餐點 Provider
final filteredMealsProvider = Provider((ref) {
    final meals = ref.watch(mealsProvider);
    final filters = ref.watch(filtersProvider);

    return meals.where((meal) {
        if (filters[Filter.GlutenFree] && !meal.isGlutenFree) {
            return false;
        }
        if (filters[Filter.LactoseFree] && !meal.isLactoseFree) {
            return false;
        }
        if (filters[Filter.Vegetarian] && !meal.isVegetarian) {
            return false;
        }
        if (filters[Filter.Vegan] && !meal.isVegan) {
            return false;
        }
        return true;
    }).toList();
});
```