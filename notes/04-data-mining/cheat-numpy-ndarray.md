---
name: cheat-numpy-ndarray
description: M7–M9 NumPy 速查——ndarray 與 broadcasting、index/slice/mask/fancy indexing、axis 方向、統計運算元、concatenate vs stack、astype 型別轉換
metadata:
  type: cheat
book: 成為AI科學家_資料探勘
chapter: M7–M9 NumPy 套件與 ndarray 操作
---

# NumPy / ndarray 速查（M7–M9）

> 定位：Pandas 的底座。Python 科學計算的基礎套件，**底層以 C 實作，速度遠快於純 Python 迴圈**。
> ⭐ 一句話：**只要你在 NumPy 裡寫 for 迴圈，通常就是寫錯了**——改用向量化運算、broadcasting 或 mask。

## 一、ndarray 是什麼（M7）

**（通常固定大小、dtype 相同）的元素構成的 N 維容器。**

| 概念 | 說明 |
|---|---|
| **shape** | 各維度的大小，如 `(2, 3)` |
| **dtype** | 全陣列共用一種型別（`int32`／`float64`…）→ 這是它快的原因 |
| **Tensor** | 基本上就是 ndarray 的另一種說法（深度學習圈的用詞） |

NumPy 另外提供 **Masked Array**、**Matrix**，以及算術／統計／線性代數（`transpose`、`inverse`、`dot`）運算。

### 🕳️ 最大的坑：切片是 **view 不是 copy**

```python
x = np.array([[1, 2, 3], [4, 5, 6]])
y = x[:, 1]      # 取中間那欄
y[0] = 9         # 改 y …
x                # …x 跟著變成 [[1, 9, 3], [4, 5, 6]]
```

⚠️ 要獨立副本必須 `.copy()`。Pandas 的 `SettingWithCopyWarning` 同源。

### 基本運算

| 寫法 | 意思 |
|---|---|
| `A + B`、`A - B`、`A / B` | element-wise |
| **`A * B`** | ⚠️ **element-wise 相乘，不是矩陣乘法** |
| **`A @ B`** / `A.dot(B)` | **矩陣乘法**（dot product） |
| `np.arange(9).reshape(3,3)` | 產生序列再重塑形狀 |

數學函數：`exp`／`sqrt`／`sin`·`cos`／`log`／`round`。

### ⭐ Broadcasting

**維度不合時，NumPy 自動把小的那個「撐開」對齊**，不必手動複製：

```python
a = np.array([1.0, 2.0, 3.0])
a * 2.0          # 純量被視為等長向量 → [2., 4., 6.]

a4x3 = np.array([[0.,0.,0.],[10.,10.,10.],[20.,20.,20.],[30.,30.,30.]])
a4x3 + np.array([1.,2.,3.])   # (4,3) + (3,) → 每一列都加上該向量
```

⭐ 這就是「整欄減平均值」「整表除以總和」這類標準化動作不必寫迴圈的原因。

## 二、取值（M8）

| 方式 | 例 | 結果 |
|---|---|---|
| 單值 | `x[2]` | 第 3 個 |
| **負索引** | `x[-2]` | 倒數第 2 個 |
| 多維 | `x[1, 3]`、`x[1, -1]` | 逗號分維度（不是 `x[1][3]`） |
| **切片** `start:stop:step` | `x[2:5]`／`x[1:7:2]`／`x[:-7]` | stop **不含** |
| **多維切片** | `y[1:5:2, ::3]` | 每維各自一組 slice |
| **Fancy Indexing** | `x[np.array([3, 3, 1, 8])]` | 用**陣列當索引**，可重複取、可亂序 |
| Fancy 二維索引 | `x[np.array([[1,1],[2,3]])]` | ⭐ 結果的 **shape 跟著索引陣列走**（可藉此改形狀） |

### Mask（Masked Array）

用**與原陣列同 shape 的 0/1 陣列**遮蔽特定元素，**被 mask 掉的不參與計算**：

```python
import numpy.ma as ma
x  = np.array([1, 2, 3, -1, 5])
mx = ma.masked_array(x, mask=[0, 0, 0, 1, 0])
mx.mean()        # 2.75 ＝ (1+2+3+5)/4，-1 不算
```

⭐ 用途：**排除無效值（-1／NaN／哨兵值）但不刪掉它們**，保住原陣列的位置對齊。這跟「刪列」是兩種思路——刪列會讓多個陣列的索引對不上。

### ⭐ Axis（最常記錯的一件事）

| | 方向 | 直覺 |
|---|---|---|
| **`axis=0`** | **縱向，沿著 rows** | 由上往下壓扁 → **每一欄得到一個數** |
| **`axis=1`** | **橫向，沿著 columns** | 由左往右壓扁 → **每一列得到一個數** |

```python
a = np.arange(0, 6).reshape([2, 3])   # [[0,1,2],[3,4,5]]
np.sum(a, axis=0)   # array([3, 5, 7])   ← 三欄各自加總
np.sum(a, axis=1)   # array([3, 12])     ← 兩列各自加總
```

⭐ 記法：**axis 指的是「被消滅掉的那個軸」**。`axis=0` 把列消掉，所以留下欄。
（Pandas 同義，M10 會再講一次；`axis=0` 有 `'index'`／`'rows'` 兩個別名，`axis=1` 只有 `'columns'`。）

### 統計運算元

| 類 | 函數 |
|---|---|
| **順序統計** | `amin`、`amax`、**`ptp`**（peak-to-peak ＝ max−min ＝ range）、`percentile`、`quantile` |
| **均值與變異** | `median`、`average`、`mean`、`std`、`var` |
| **關聯** | **`corrcoef`**（相關係數矩陣）、`correlate`、`cov` |
| **直方圖** | `histogram`、`histogram2d`、`bincount` |

範例檔：`NDarrayIntro.ipynb`

## 三、合併與轉換（M9）

### ⭐ concatenate vs stack（考試／面試常混）

| | `np.concatenate((a1, a2), axis=0)` | `np.stack(arrays, axis=0)` |
|---|---|---|
| 在哪合 | **沿現有的軸** | ⭐ **在新軸上疊** |
| 維度 | **不變** | **+1** |
| 直覺 | 把兩張表接起來 | 把 N 張表疊成一疊 |

```python
# concatenate：(3,3) + (3,3)
np.concatenate((A, B), axis=0)   # → (6,3) 縱向接（default）
np.concatenate((A, B), axis=1)   # → (3,6) 橫向接

# stack：10 個 (3,4) 陣列
np.stack(arrays, axis=0).shape   # (10, 3, 4)  ← 新軸插在最前
np.stack(arrays, axis=1).shape   # (3, 10, 4)
np.stack(arrays, axis=2).shape   # (3, 4, 10)

a = np.array([1,2,3]); b = np.array([2,3,4])
np.stack((a, b))            # [[1,2,3],[2,3,4]]
np.stack((a, b), axis=-1)   # [[1,2],[2,3],[3,4]]  ← 配對成 pairs
```

⭐ 判準：**要「更長／更寬」用 concatenate；要「多一層」（時間序列堆成 batch、多張圖疊成一個 tensor）用 stack。**

### astype 型別轉換

```python
ndarray.astype(dtype, order='K', casting='unsafe', subok=True, copy=True)
```

| 參數 | 選項 | 說明 |
|---|---|---|
| **`dtype`** | `int`／`float`／`str`… | 目標型別 |
| **`order`** | `'C'`／`'F'`／`'A'`／`'K'` | 記憶體順序：**C ＝ row 優先**、**F ＝ Fortran，column 優先** |
| **`casting`** | `'no'`／`'equiv'`／`'safe'`／`'same_kind'`／**`'unsafe'`（預設）** | 轉換的「積極程度」 |

🕳️ **預設是 `unsafe`＝不會攔你**：`np.array([1, 2, 2.5]).astype(int)` → `[1, 2, 2]`，**2.5 被無聲截斷（不是四捨五入）**。
⚠️ 金額／比率欄轉 int 前先想清楚要 `round()` 還是 `floor()`；要 NumPy 幫忙把關就設 `casting='safe'`。

## 對接

- **多資產價格序列**：`stack` 把多檔／多期的價格序列疊成三維、`corrcoef` 算相關矩陣（分散的前提是低相關）；mask 用來排除停牌／無交易日而不破壞日期對齊。
- ⚠️ Mask 與離群值的關係：**mask 掉 ≠ 該刪**，判斷仍照 [framework-離群值與資料轉換](framework-離群值與資料轉換.md) 的「先問為什麼有這個點」。

## 相關

[MOC-資料探勘](MOC-資料探勘.md) · [framework-eda探索流程](framework-eda探索流程.md) · [framework-離群值與資料轉換](framework-離群值與資料轉換.md) · [cheat-pandas清洗與轉換](cheat-pandas清洗與轉換.md) · [MOC-python語法速查](../01-python/MOC-python語法速查.md)
