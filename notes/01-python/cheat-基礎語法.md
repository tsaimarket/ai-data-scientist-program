---
name: cheat-基礎語法
description: Python keyword 保留字、identifier 命名規則、literal 字面值種類、註解/多行/縮排、print·input 速查
metadata:
  type: cheat
book: 快速闖關Python語法世界
chapter: M3
---

# 基礎語法速查（keyword / identifier / literal / 結構）

## keyword（保留字）

特殊用途、不可當變數名。查表：
```python
import keyword
keyword.kwlist          # 列出全部保留字
keyword.iskeyword('if') # True；'IF' → False（大小寫有別）
```
常見：`and as assert break class continue def del elif else except finally for from global if import in is lambda not or pass raise return try while with yield` + `True False None async await nonlocal`。

## identifier（識別字命名規則）

變數/函數/類別的名字。規則：

| 規則 | 說明 |
|---|---|
| 可用字元 | `A-Z a-z 0-9 _` |
| 開頭 | **不能以數字開頭** |
| 大小寫 | 有別（`a` ≠ `A`） |
| 保留字 | 強烈建議不要用 keyword |

驗證：`"abc".isidentifier() → True`；`"99a" → False`；`"_" → True`；`"for".isidentifier()` 雖回 True 但語意上不可用。

| 範例 | 合法? |
|---|---|
| `ab10c` `abc_DE` `_` `_abc` | ✅ |
| `99` `x+y` `for` `a@` `9abc` | ❌ |

## literal（字面值）

| 類別 | 例 |
|---|---|
| Numeric | 十進 `30`；二進 `0b11001`；八進 `0o156`；十六進 `0x12` |
| String | `'單'` / `"雙"` 引號；逃脫字元 `\n \t \\ \'` |
| Boolean | `True` / `False` |
| Special | `None` |

## 程式結構

| 概念 | 寫法 |
|---|---|
| 敘述 statement | 一行一敘述，換行即結尾（不需 `;`） |
| 單行註解 | `# 註解` |
| 多行註解 | 一對 `'''...'''`（三引號） |
| 多行敘述 | 行尾 `\` 把多行視為同一行 |
| 程式區塊 | **縮排** 表示；同縮排＝同區塊；慣用 4 空白；**不可混用空白與 tab**（會錯） |

## print / input

```python
print('Hello', 'World', sep='-', end='\n')  # sep 分隔、end 結尾可改
name = input('請輸入：')   # 永遠回傳「字串」，要數字得自己 int()/float() 轉
```

延伸：運算與賦值見 [framework-運算元與優先序](framework-運算元與優先序.md)；型別見 [cheat-資料型別總覽](cheat-資料型別總覽.md)。
