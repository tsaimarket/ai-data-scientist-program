---
name: cheat-list-tuple-set-dict
description: list/tuple/set/dict 四容器型別的特性對照與常見內建方法速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M6,M7
---

# 容器型別速查（list / tuple / set / dict）

## 四容器一覽

| 型別 | 符號 | 有序 | 可變 | 可重複 | 索引 |
|---|---|---|---|---|---|
| list 列表 | `[ ]` | ✅ | ✅ | ✅ | ✅ |
| tuple 元組 | `( )` | ✅ | ❌ | ✅ | ✅ |
| set 集合 | `{ }` | ❌ | ✅ | ❌（自動去重） | ❌ |
| dict 字典 | `{k:v}` | ✅(Py3.7+) | ✅ | key 不可重複 | 用 key |

元素可混型別：`[1,'a',3.0]`、`('Hello',33,'55')` 皆可。

## list 常見方法

| 方法 | 作用 |
|---|---|
| `append(x)` | 尾端加一個 |
| `extend(iter)` | 尾端加多個（攤平） |
| `insert(i,x)` | 指定位置插入 |
| `pop(i)` | 移除並回傳指定位置（預設最後） |
| `remove(x)` | 移除第一個值為 x 的元素 |
| `clear()` | 清空 |
| `index(x)` | 第一個 x 的索引 |
| `count(x)` | x 出現次數 |
| `sort()` / `reverse()` | 排序 / 反轉（原地） |
| `copy()` | 淺拷貝 |

```python
L = ['Hello','Hi','Hey']
L[0]      # 'Hello'   也支援負索引/切片，與字串相同
```

## tuple

不可變 → 沒有 append/sort 等修改方法；用於「不該被改」的資料、當 dict 的 key、函數多回傳值。單元素要逗號：`(5,)`。

## set

```python
s = {1, 2, 3}
s.add(4); s.discard(2)
a & b   # 交集     a | b   # 聯集
a - b   # 差集     a ^ b   # 對稱差
```
用途：去重、成員測試（`in` 超快）、集合運算。

## dict（鍵值對）

| 操作 | 作用 |
|---|---|
| `d[key]` | 取值（不存在報 KeyError） |
| `d.get(key, 預設)` | 取值（不存在回預設，安全） |
| `d[key] = v` | 新增/更新 |
| `del d[key]` | 刪除 |
| `key in d` | 是否有此 key |
| `d.keys()/.values()/.items()` | 鍵/值/鍵值對 |
| `len(d)` | 數量 |
| `iter(d)` | 以 key 建迭代器 |

```python
for k, v in d.items():   # 最常用的走訪
    print(k, v)
```

延伸：用 `for` 迭代容器見 [framework-流程控制與迴圈](framework-流程控制與迴圈.md)；`map/filter` 處理容器見 [framework-函數](framework-函數.md)。
