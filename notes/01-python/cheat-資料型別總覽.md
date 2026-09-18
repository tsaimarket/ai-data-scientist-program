---
name: cheat-資料型別總覽
description: Python 型別分類（數值/布林/序列/雜湊）、mutable vs immutable、虛數、型別轉換函數速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M5,M7
---

# 資料型別總覽（含 mutable / 型別轉換）

## 型別分類

| 類別 | 型別 | 例 |
|---|---|---|
| 數值 | `int` | `2, 5, 987`（支援 `0b`/`0o`/`0x` 前綴） |
| 數值 | `float` | `2.34`, `5.67e-3`（＝5.67×10⁻³） |
| 數值 | `complex` 虛數 | `8+9j`（`j` 為虛部，`5.j`、`.3j` 皆可） |
| 布林 | `bool` | `True(1)` / `False(0)` |
| 序列 | `str` / `list` / `tuple` | 見 [cheat-字串](cheat-字串.md)、[cheat-list-tuple-set-dict](cheat-list-tuple-set-dict.md) |
| 雜湊 | `set` / `dict` | 見 [cheat-list-tuple-set-dict](cheat-list-tuple-set-dict.md) |

## Mutable vs Immutable（最常踩的坑）

| | Mutable（可變） | Immutable（不可變） |
|---|---|---|
| 型別 | `list` `set` `dict` | `numeric` `str` `tuple` |
| 傳遞 | pass by reference（傳參考，函數內改會影響外部） | pass by value（傳值/物件位址，改會生新物件） |

→ 預設參數別用 mutable（`def f(x, lst=[])` 是經典 bug）；要拷貝用 `list.copy()` / `copy.deepcopy()`。

## 布林與真假值

布林只有 `True`/`False`，常用於流程判斷。空值視為 False：`0`、`''`、`[]`、`{}`、`None` → falsy。

## 型別轉換函數

```python
int(x)   float(x)   complex(x)     # 轉數值
str(x)   list(x)    tuple(x)       # 轉序列
set(x)   dict(x)                   # 轉雜湊
```
常見：`int('5')→5`、`int(3.9)→3`（截斷非四捨五入）、`list('abc')→['a','b','c']`、`set([1,1,2])→{1,2}`（去重）、`type(x)` 查型別。

延伸：運算/身分 `is` 見 [framework-運算元與優先序](framework-運算元與優先序.md)。
