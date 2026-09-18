---
name: framework-函數
description: 自訂函數/預設值/*args/**kwargs/多回傳值、內建函數清單、全域 vs 區域變數、lambda+filter/map/reduce 速查
metadata:
  type: framework
book: 快速闖關Python語法世界
chapter: M11-12
---

# 函數速查（自訂 / 內建 / lambda）

## 自訂函數

```python
def greet(name, msg='Hi'):     # name=必填參數, msg=預設值參數
    return f'{msg}, {name}'

greet('Tom')           # 'Hi, Tom'
greet('Tom', 'Hello')  # 'Hello, Tom'
```
- 命名規則同 identifier（不可數字開頭）。
- 參數：`parameter`＝定義時的形參、`argument`＝呼叫時的實參。

### 變動參數

| 寫法 | 接收 | 型別 |
|---|---|---|
| `*args` | 不定個數的位置參數 | tuple |
| `**kwargs` | 不定個數的關鍵字參數 | dict |

```python
def f(*args, **kwargs):
    print(args)      # (1, 2, 3)
    print(kwargs)    # {'a': 10}
f(1, 2, 3, a=10)
```

### 多回傳值

```python
def minmax(lst):
    return min(lst), max(lst)   # 實為回傳 tuple
lo, hi = minmax([3,1,9])        # 解包
```

## 全域 vs 區域變數

| 種類 | 範圍 |
|---|---|
| 全域 global | 任何區塊都能讀取 |
| 區域 local | 只在函數區塊內存在 |

函數內要**改**全域變數須 `global x` 宣告，否則賦值會建立同名區域變數。

## 內建函數（常用）

`print input len range type` · `int float str list tuple set dict bool` · `abs round pow divmod` · `min max sum sorted` · `enumerate zip map filter` · `id isinstance` · `open` · `help dir`。

```python
for i, v in enumerate(['a','b']):  # (0,'a') (1,'b')
    ...
list(zip([1,2],[3,4]))             # [(1,3),(2,4)]
sorted(lst, key=lambda x: x[1], reverse=True)
```

## lambda 匿名函數

一行定義函數：`lambda 參數: 運算式`。

```python
sq = lambda x: x**2
# 配 filter：篩選符合條件
list(filter(lambda x: x > 2, [1,2,3,4]))      # [3,4]
# 配 map：每元素轉換
list(map(lambda x: x*2, [1,2,3]))             # [2,4,6]
# 配 reduce：累積合併（需 import）
from functools import reduce
reduce(lambda a,b: a+b, [1,2,3,4])            # 10
```

| 函數 | 作用 |
|---|---|
| `filter(f, seq)` | 留下 `f(x)` 為 True 的元素 |
| `map(f, seq)` | 每元素套 `f` |
| `reduce(f, seq)` | 兩兩合併成單一值 |

延伸：把函數封進物件見 [framework-物件導向](framework-物件導向.md)；遞迴（函數呼叫自己）見 [concept-遞迴與動態規劃](concept-遞迴與動態規劃.md)。
