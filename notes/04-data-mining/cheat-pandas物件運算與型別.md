---
name: cheat-pandas物件運算與型別
description: M10 Pandas 物件運算速查——DataFrame 的 axis（與 ndarray 同一個口訣）、Pandas/Python/NumPy 三層 dtype 對應、改型別三種做法與 read_csv 的 dtype/converters、重複值 duplicated() 取反、⚠️ np.isnan 抓不到 inf 要用 masked_invalid
metadata:
  type: cheat
book: 成為AI科學家_資料探勘
chapter: M10 Pandas 物件運算（p109–120）
---

# Pandas 物件運算速查（M10）

> 定位：M7–M9 講的是 **ndarray** 的運算，M10 把同一套觀念搬到 **DataFrame**。
> ⚠️ 本模組的遺失值段落，講義自己就註明「**請參考模組：資料清洗與轉換（I）**」→ 見 [cheat-pandas清洗與轉換](cheat-pandas清洗與轉換.md)，這裡只記**不重複的那幾格**。

## 一、DataFrame 的 Axis（與 ndarray 同一個口訣）

DataFrame 像 SQL table 有行與列，**每一欄是一個 Series**。

| 軸 | 方向 | 別名 |
|---|---|---|
| `axis=0` | **縱（列）向** | ⭐ **有兩個**：`'index'`、`'rows'` |
| `axis=1` | **橫（行）向** | 只有一個：`'columns'` |

```python
df = pd.DataFrame({'a': pd.Series([10,30,60,80,90]),
                   'b': pd.Series([22,44,55,77,101])})

df.sum(axis=0)   # 縱向加總 → a 270, b 299   （每一欄一個數）
df.sum(axis=1)   # 橫向加總 → 32,74,115,157,191（每一列一個數）
```

> ⭐ **口訣完全沿用 [cheat-numpy-ndarray](cheat-numpy-ndarray.md)：axis ＝ 被消滅的那個軸。**
> `axis=0` 把「列」吃掉 → 剩下每一欄一個數；`axis=1` 把「欄」吃掉 → 剩下每一列一個數。
> 🕳️ 這是全課兩個「**記反不會報錯、只會全錯**」的坑之一（另一個是 M6 的 Lift≈1）。

### 定位取值

```python
df.loc[2, 'b']   # 第 2 列、'b' 欄 → 'blue'
df.loc[3, 'a']   # → 8
```

⚠️ `.loc[列, 欄]`——**列在前、欄在後**，跟 axis 編號同序（0 是列、1 是欄）。

## 二、Pandas / Python / NumPy 三層 dtype 對應

| Pandas dtype | Python type | NumPy type | 用在 |
|---|---|---|---|
| `object` | str or mixed | `string_`, `unicode_`, mixed | 文字，或**數值與非數值混在一起** |
| `int64` | int | `int_`, `int8/16/32/64`, `uint8/16/32/64` | 整數 |
| `float64` | float | `float_`, `float16/32/64` | 浮點數 |
| `bool` | bool | `bool_` | True/False |
| `datetime64` | — | `datetime64[ns]` | 日期時間 |
| `timedelta[ns]` | — | — | 兩個 datetime 的差 |
| `category` | — | — | **有限的文字值清單** |

> ⭐ 講師口述的排序：**NumPy 最複雜**（ndarray 需固定型別，還要考慮 precision）、**Python 最靈活**、Pandas 居中。
> 🕳️ 實務上最常中的一格是 **`object`**——只要一欄裡混進一個 `'N/A'` 或 `'1,234'`，整欄就掉成 object，`sum()` 會變字串串接而不是報錯。

## 三、改型別的三種做法

| 做法 | 用在 |
|---|---|
| `astype()` | 型別乾淨、只是要換 |
| **自訂函數**（converters） | 需要先剝掉符號再轉（`$1,234` → `1234`） |
| `to_numeric()` / `to_datetime()` | Pandas 內建轉換，配 `errors=` 控制失敗行為 |

### ⭐ 讀檔當下就轉：`read_csv` 的 `dtype` 與 `converters`

```python
df_2 = pd.read_csv("sales_data_types.csv",
    dtype={'Customer Number': 'int'},
    converters={
        '2016': convert_currency,          # 自訂函數：剝掉 $ 與 ,
        '2017': convert_currency,
        'Percent Growth': convert_percent, # 自訂函數：剝掉 % 並除以 100
        'Jan Units': lambda x: pd.to_numeric(x, errors='coerce'),
        'Active':    lambda x: np.where(x == "Y", True, False)})
```

| 參數 | 差別 |
|---|---|
| `dtype=` | **直接指定**欄位型別（值本身已乾淨） |
| `converters=` | **先過一個函數**再進 DataFrame（值需要清洗） |

> ⭐ `errors='coerce'`＝**轉不動的就變成 NaN**（而不是丟例外）→ 讀完馬上 `isnull().sum()` 就知道髒了幾筆。
> ⚠️ 但這也意味著**錯誤被靜默吞掉**：`coerce` 之後一定要看一眼 NaN 數量，否則髒資料會偽裝成缺失值。

## 四、NumPy 端的遺失值（Pandas 之外的那一半）

```python
np.isnan(np.nan)                       # True
np.isnan(np.inf)                       # ⚠️ False ——infinity 不是 NaN
np.isnan([np.log(-1.), 1., np.log(0)]) # [True, False, False]
```

🕳️ **`np.log(0)` 產生 `-inf`，`np.isnan()` 抓不到它。** 只清 NaN 的流程會把 inf 原封不動留在資料裡，後面一算全毀。

```python
x  = np.array([1.0, 2.5, np.nan, 1.3, np.inf, 7.2])
xm = np.ma.masked_invalid(x)   # ⭐ NaN 與 inf 一起遮
# [1.0 2.5 -- 1.3 -- 7.2]
```

masked array 的運算會把被遮的位置保持為 `--`，且**除以 0 也自動變成遮蔽值**：

```python
x = np.ma.array([1, 2, 3], mask=[False, False, True])
y = np.ma.array([1, 0, 1])
print(x * y)   # [1 0 --]
print(x / y)   # [1.0 -- --]   ← 除以 0 那格也被遮起來
```

> 📌 呼應 [cheat-numpy-ndarray](cheat-numpy-ndarray.md) 的 mask 段：**遮蔽但不刪除，索引對齊不會跑掉。**

## 五、重複值處理

```python
bool_series = data["First Name"].duplicated()  # 回一個布林 Series
data[bool_series]                              # 看重複的那些
data = data[~bool_series]                      # ⭐ 取反（NOT）→ 留下非重複
data.info()
```

| 要點 | 說明 |
|---|---|
| `duplicated()` 回傳的是 **布林 Series** | 不是資料本身，要拿它去索引 |
| ⭐ `~` **取反** | 保留非重複列的標準寫法 |
| ⚠️ **NaN 也會被視為重複** | 兩個空值會被判定為相同 → **先處理遺失值再去重**，否則會誤刪 |
| 預設只標**第二次以後**出現的 | 第一次出現的那筆是 `False`（會被保留） |

> 🔴 順序很重要：**先填補／處理 NaN → 再去重**。反過來做，一堆本來不同的列會因為都缺值而被當成重複砍掉。

## 相關

- [cheat-numpy-ndarray](cheat-numpy-ndarray.md)（M7–M9，同一套 axis 與 mask 觀念的底座）
- [cheat-pandas清洗與轉換](cheat-pandas清洗與轉換.md)（M3，遺失值偵測與填補的本體）
- [cheat-pandas載入資料](cheat-pandas載入資料.md)（M2，`read_csv` 的其他參數）
- [framework-離群值與資料轉換](framework-離群值與資料轉換.md)（M4，去重之後的形狀調整）
- [MOC-資料探勘](MOC-資料探勘.md)
